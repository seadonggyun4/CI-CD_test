# CI/CD_TEST 파이프라인


## Git Action 워크플로우
1. 저장소를 체크아웃 합니다
2. Node.js 18.x 버전을 설정합니다.
3. 프로젝트 의존성을 설치합니다.
4. Next.js 프로젝트를 빌드합니다.
5. AWS 자격 증명을 구성합니다.
6. 빌드된 파일을 S3 버킷에 동기화합니다.
7. CloudFront 캐시를 무효화합니다.

<br>

## 주요 링크
- S3 버킷 웹사이트 엔드포인트: http://hanghea.s3-website.us-east-2.amazonaws.com
- CloudFrount 배포 도메인 이름: https://dykx6yakigdlf.cloudfront.net

<br>

## 주요 개념
- GitHub Actions과 CI/CD 도구: Git Action, s3, cloudFront
- S3와 스토리지: _________
- CloudFront와 CDN: _________
- 캐시 무효화(Cache Invalidation): _________
- Repository secret과 환경변수: 
  - AWS_ACCESS_KEY_ID
  - AWS_REGION
  - AWS_SECRET_ACCESS_KEY
  - CLOUDFRONT_DISTRIBUTION_ID
  - S3_BUCKET_NAME