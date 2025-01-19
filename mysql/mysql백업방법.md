Windows 환경에서 MySQL 데이터베이스를 백업하고 다른 컴퓨터에서 사용하는 방법은 아래 단계를 따르세요.

---

### 1. **`mysqldump` 명령 실행 준비**
   - Windows에서 MySQL의 `mysqldump`를 실행하려면 MySQL이 설치된 경로를 알아야 합니다.
   - 기본 설치 경로는 보통 다음과 같습니다:
     ```
     C:\Program Files\MySQL\MySQL Server 8.0\bin
     ```
   - `bin` 폴더의 경로를 시스템 `PATH` 환경 변수에 추가하면 명령어를 어디서든 실행할 수 있습니다.

---

### 2. **데이터베이스 백업**
   **방법 1: `cmd`에서 실행**
   1. 명령 프롬프트(cmd)를 열고 MySQL 설치 경로로 이동합니다:
      ```cmd
      cd "C:\Program Files\MySQL\MySQL Server 8.0\bin"
      ```
   2. `mysqldump` 명령을 실행합니다:
      ```cmd
      mysqldump -u [username] -p [database_name] > C:\backup_file.sql
      ```
      예:
      ```cmd
      mysqldump -u root -p my_database > C:\Users\username\Desktop\my_database_backup.sql
      ```
      - `-u`: MySQL 사용자 이름 (보통 `root`).
      - `-p`: 비밀번호 입력을 요구.
      - `[database_name]`: 백업할 데이터베이스 이름.
      - `[backup_file.sql]`: 생성할 백업 파일 경로.

   **방법 2: MySQL Workbench 사용**
   1. MySQL Workbench를 엽니다.
   2. 메뉴에서 **Server > Data Export**를 선택합니다.
   3. 백업할 데이터베이스를 선택하고, **Dump Structure and Data**를 클릭합니다.
   4. **Export to Self-Contained File**을 선택하고 저장 경로를 설정한 후 **Start Export**를 클릭합니다.

---

### 3. **백업 파일 이동**
   생성된 `backup_file.sql`을 다른 컴퓨터로 이동합니다.
   - USB 드라이브, 네트워크 공유 폴더, 클라우드 스토리지(예: Google Drive) 또는 파일 전송 도구를 사용하세요.

---

### 4. **다른 컴퓨터에서 데이터베이스 복원**
   새 컴퓨터에서 데이터베이스를 복원하려면 아래 단계를 따릅니다:

   **방법 1: `cmd`에서 복원**
   1. 명령 프롬프트(cmd)를 열고 MySQL 설치 경로로 이동합니다:
      ```cmd
      cd "C:\Program Files\MySQL\MySQL Server 8.0\bin"
      ```
   2. MySQL에 데이터베이스를 생성합니다:
      ```sql
      mysql -u [username] -p
      CREATE DATABASE [database_name];
      ```
   3. 복원 명령 실행:
      ```cmd
      mysql -u [username] -p [database_name] < C:\backup_file.sql
      ```
      예:
      ```cmd
      mysql -u root -p my_database < C:\Users\username\Desktop\my_database_backup.sql
      ```

   **방법 2: MySQL Workbench 사용**
   1. MySQL Workbench를 엽니다.
   2. **Server > Data Import**를 선택합니다.
   3. **Import from Self-Contained File**을 선택하고 `backup_file.sql` 경로를 지정합니다.
   4. **Default Target Schema**에 복원할 데이터베이스 이름을 선택하거나 생성합니다.
   5. **Start Import**를 클릭합니다.

---

### 5. **복원 확인**
   복원이 완료되었는지 확인하려면 MySQL에 접속하고 다음 명령을 실행합니다:
   ```sql
   SHOW DATABASES;
   USE [database_name];
   SHOW TABLES;
   ```

---

### 팁
1. **권한 문제**: Windows에서는 명령어를 실행할 때 관리자 권한이 필요할 수 있습니다.
2. **MySQL 버전 확인**: 백업과 복원 시 MySQL 버전이 같거나 호환되는지 확인하세요.
3. **전체 데이터베이스 백업**: `--all-databases` 옵션으로 모든 데이터베이스를 백업/복원할 수 있습니다:
   ```cmd
   mysqldump -u root -p --all-databases > all_databases_backup.sql
   mysql -u root -p < all_databases_backup.sql
   ```

필요한 추가 정보가 있으면 언제든지 물어보세요! 😊