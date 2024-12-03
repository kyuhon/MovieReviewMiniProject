# :movie_camera: Movie Review Project
영화 리뷰 관리 사이트 제작

---


## :black_nib: 프로젝트 소개
사용자, 영화, 시청기록, 리뷰 테이블을 생성해서 <br> 
특정 영화에 대한 시청기록이 존재하는 사용자만이 리뷰를 작성할 수 있습니다.


## :golf: 개발 환경
- `Java`
- `JDK-21.0.4`
- **IDE** : Eclipse
- **Database**: Oracle DB (23c Express Edition)
- **SQL Developer**: Version 23.1

## :key: ERD
<img src ="https://github.com/chan237/MovieReviewProject/blob/main/MovieReviewProject(1)/test/1.png">

## :bulb: 주요 기능

#### USER_ID_TRIGGER
```
CREATE OR REPLACE TRIGGER USER_ID_TRIGGER
BEFORE INSERT ON Users
FOR EACH ROW
BEGIN
    IF :NEW.user_id IS NULL THEN
        :NEW.user_id := 'USER' || TO_CHAR(USER_ID_SEQ.NEXTVAL, 'FM0000');
    END IF;
END;
/
```
- 유저 아이디 자동 생성

#### FIND_PASSWORD_FUNC
```
CREATE OR REPLACE FUNCTION FIND_PASSWORD_FUNC (
    p_user_id IN VARCHAR2,
    p_email IN VARCHAR2
) RETURN VARCHAR2
AS
    v_password VARCHAR2(255);
BEGIN
    SELECT password
    INTO v_password
    FROM Users
    WHERE user_id = p_user_id
      AND email = p_email;
    RETURN v_password;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RETURN '존재하지 않는 사용자 입니다';
END;
/
```
- 아이디와 이메일 입력시 패스워드 출력

#### LIST_MOVIES_BY_GENRE
```
CREATE OR REPLACE PROCEDURE list_movies_by_genre (
    p_genre IN VARCHAR2,
    p_cursor OUT SYS_REFCURSOR
) IS
BEGIN
    OPEN p_cursor FOR
        SELECT title, release_date, duration
        FROM Movies
        WHERE genre = p_genre;
END;
/
```
- 장르별 영화 출력

## :mag_right: 화면 출력
> 회원 메뉴
> <img src="https://github.com/chan237/MovieReviewProject/blob/main/MovieReviewProject(1)/test/2.png">

> 영화 메뉴
> <img src="https://github.com/chan237/MovieReviewProject/blob/main/MovieReviewProject(1)/test/3.png">

> 시청 기록 메뉴
> <img src="https://github.com/chan237/MovieReviewProject/blob/main/MovieReviewProject(1)/test/4.png">

> 리뷰 메뉴
> <img src="https://github.com/chan237/MovieReviewProject/blob/main/MovieReviewProject(1)/test/5.png">
