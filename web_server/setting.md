---

# 🧾 **임차효율표 설정 문서 – 로그 파트**

---

## 📂 **1. Logrotate 설정**

### **📌 설정 파일 경로**

```
/etc/logrotate.d/pm2
```

### **📌 설정 파일 내용**

```
/home/ubuntu/.pm2/logs/rental/*.log {
        size 50M
        rotate 10
        compress
        delaycompress
        missingok
        notifempty
        copytruncate
        dateext
        dateformat -%Y%m%d-%s
        su ubuntu ubuntu
}
```

### **📘 설명**

- **size 50M** → 로그 크기 50MB 이상일 때 회전
- **rotate 10** → 최대 10개의 로그 백업 유지
- **compress / delaycompress** → 이전 로그 압축
- **missingok** → 로그 없어도 에러 없이 통과
- **notifempty** → 비어있는 로그는 회전 안 함
- **copytruncate** → 서비스 중단 없이 로그 잘라냄
- **dateext** → 날짜 기반으로 백업 로그 생성
- **su ubuntu ubuntu** → ubuntu 권한으로 로그 회전 처리

---

## 📂 **2. PM2 설정 파일 (임차효율표 Node 서버)**

### **📌 파일명**

```
rental.config.cjs
```

### **📌 설정 내용**

```jsx
module.exports = {
  apps: [
    {
      name: 'rentalEfficiencyNode',
      script: './src/Main.js',
      instances: 1,
      exec_mode: 'fork',
      watch: true,
      error_file: '/home/ubuntu/.pm2/logs/rental/my-service-error.log',
      out_file: '/home/ubuntu/.pm2/logs/rental/my-service-out.log',
      log_date_format: 'YYYY-MM-DD HH:mm:ss',
    }
  ]
};
```

### **📘 설명**

- **name**: PM2 프로세스 이름
- **script**: Node 진입 파일
- **instances**: 실행 인스턴스 수 (1개)
- **exec_mode**: 실행 모드 (fork)
- **watch**: 파일 변경 감지 후 자동 재시작
- **error_file / out_file**: 커스텀 로그 저장 경로
- **log_date_format**: 로그 생성 시 날짜 포맷

---

## 📂 **3. 로그 저장 위치**

### **📌 디렉토리 경로**

```
/home/ubuntu/.pm2/logs/rental/
```

### **📘 포함 로그 파일**

- `my-service-error.log`
- `my-service-out.log`
- logrotate에서 이 디렉토리의 모든 `.log` 파일을 관리함

---

## 🚀 **4. PM2 실행 방식**

### **📌 프로젝트 실행 경로**

```
/var/www/node/rental_efficiency
```

### **📌 PM2 실행 명령어**

프로젝트 경로로 이동 후:

```
cd /var/www/node/rental_efficiency
pm2 start rental.config.cjs
```

### **📌 자동 재시작 저장**

```
pm2 save
```

---

# 🛡️ MariaDB 백업 자동화 & 관리 시스템 문서

## 🧱 디렉토리 구조

```
/home/dev/
├── scripts/
│   └── backup_rental_db.sh         ← 백업 스크립트
├── backup/                         ← DB 백업 파일 저장 디렉토리 (.sql)
└── logs/
    └── rental_logs/
        ├── backup.log              ← 정상 동작 로그
        └── backup.err              ← 에러 발생 로그
```

---

## 🧾 백업 스크립트 (`/home/dev/scripts/backup_rental_db.sh`)

```bash
#!/bin/bash

# 안정성 확보
cd /home/dev/scripts

# DB 설정
DB_USER="DB유저이름"
DB_PASSWORD='DB비밀번호'
DB_NAME="DB명"

# 경로 설정
BACKUP_DIR="/home/dev/backup"
LOG_DIR="/home/dev/logs/rental_logs"
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
BACKUP_FILE="$BACKUP_DIR/${DB_NAME}_backup_$TIMESTAMP.sql"

# 디렉토리 보장
mkdir -p "$BACKUP_DIR"
mkdir -p "$LOG_DIR"

# 백업 실행
mysqldump -u$DB_USER -p$DB_PASSWORD $DB_NAME > "$BACKUP_FILE"

# 결과 로그 기록
if [ $? -eq 0 ]; then
  echo "[`date`] ✅ 백업 성공: $BACKUP_FILE" >> "$LOG_DIR/backup.log"
else
  echo "[`date`] ❌ 백업 실패: $BACKUP_FILE" >> "$LOG_DIR/backup.err"
fi

# 7일 이상 지난 백업 삭제
find "$BACKUP_DIR" -type f -mtime +7 -name "*.sql" -exec rm -f {} \;

# 14일 이상 지난 로그 삭제
find "$LOG_DIR" -type f -mtime +14 -name "*.log" -exec rm -f {} \;
find "$LOG_DIR" -type f -mtime +14 -name "*.err" -exec rm -f {} \;
```

---

## 🕒 Crontab 설정 (자동 백업 스케줄링)

```cron
# ==== 임차효율표 DB 백업 (매일 새벽 2시) ====
0 2 * * * /home/dev/scripts/backup_rental_db.sh
```

### 크론탭 등록
```bash
crontab -e
```

### 크론탭 확인
```bash
crontab -l
```

---

## ✅ 수동 테스트 방법

```bash
bash /home/dev/scripts/backup_rental_db.sh
```

### 결과 확인
```bash
# 백업 파일
ls /home/dev/backup/*.sql

# 성공 로그
tail -n 20 /home/dev/logs/rental_logs/backup.log

# 에러 로그
tail -n 20 /home/dev/logs/rental_logs/backup.err
```

---

## 💡 운영 꿀팁

| 항목 | 내용 |
|------|------|
| mysqldump 특수문자 | DB_PASSWORD은 `'비밀번호'`처럼 작은따옴표로 감싸기 |
| `-type -f` → ❌ | `-type f`로 정확하게 입력할 것 |
| 경로는 절대경로 | 크론에서 `../`는 깨질 수 있음 |
| 로컬에서 테스트 후 등록 | 크론 전에 반드시 `bash script.sh`로 확인 |
| `cron` 로그 확인 | `grep CRON /var/log/syslog` (Ubuntu 기준) |

---