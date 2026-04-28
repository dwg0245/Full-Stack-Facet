# 빌드 단계

# AS 이름 지어주기
FROM gradle:8.14.4-jdk17 AS builder

# CD 같은거
WORKDIR /app

#도커 컴포즈로 받기
ARG VITE_BACKEND_URL

# # 필요한 파일 복사 (라이브러리 복사)
COPY build.gradle ./
# 프로젝트 전체 설정 복사
COPY settings.gradle ./

# 라이브러리 다운 받기
RUN gradle dependencies --no-daemon

COPY ./src ./src
# 컨테니어 안에 jar 파일이 생성이 될거임
RUN gradle bootjar --no-daemon


#실행 단계
FROM openjdk:17-ea-17-slim
#생성한 파일을 가져와서 넣어라
#위에서 생성한 컨테이너 jar 파일을 /app.jar 안에 넣어라
COPY  --from=builder /app/build/libs/*.jar /app.jar
EXPOSE 8080
CMD ["java", "-jar", "/app.jar"]