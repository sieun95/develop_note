### Front

```bash
yarn add react-magic-slider-dots --save
```

```bash
npm install sweetalert2 --save --legacy-peer-deps
```

#### 서버 요청

`frontend/src/redux/api.js`에서 axios요청 확인

---

### Back

`.env` 파일 만들기

```tex
PRIVATE_KEY = "0xd356296163500011c5beb930b9ba117284be97330173d6c2c429ca64e888ec93"
ADMIN_ACCOUNT = "0x4eAC9d1c98F24404a90BF5495142d8294afB24F2"

HOST=13.124.136.43
USER_ID=sean
PASSWORD=Dltldms1234!
DATABASE=defiProject
```

---

### DB Table

```sql
CREATE DATABASE defiProject;

USE defiProject;

DROP TABLE gamePoint;

CREATE TABLE gamePoint(
idx INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
account VARCHAR(255) NOT NULL,
point INT NOT NULL DEFAULT 0
);

INSERT INTO gamePoint(account) VALUES('test');

SELECT * FROM gamePoint;


CREATE TABLE nftReward(
    season INT AUTO_INCREMENT PRIMARY KEY,
    rewardEdition VARCHAR(45)
);

SELECT * FROM nftReward;
```





