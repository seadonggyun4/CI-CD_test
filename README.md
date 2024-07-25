# CI/CD_TEST 파이프라인
![cl_cd_test drawio](https://github.com/user-attachments/assets/00eef9d9-2e7f-4956-9027-42f32a5ef940)

## Git Action 워크플로우
### Checkout repository
```yaml
  - name: Checkout repository
    uses: actions/checkout@v2
```
이 단계에서는 actions/checkout@v2라는 GitHub 액션을 사용해 현재 레포지토리의 모든 내용을 작업자(worker) 머신의 현재 디렉토리에 체크아웃합니다.

<br>

### Install dependencies
```yaml
  - name: Install dependencies
    run: npm ci
```
이 단계에서는 npm ci 명령을 실행하여 프로젝트의 의존성을 설치합니다. npm ci는 package-lock.json 또는 npm-shrinkwrap.json을 사용하여 의존성을 정확하게 재현하며, 더 빠른 설치를 제공합니다.

<br>

### Build
```yaml
  - name: Build
    run: npm run build
```
npm run build 명령을 사용하여 프로젝트를 빌드합니다. 이 단계에서는 build 스크립트가 package.json 파일에 정의되어 있어야 합니다. 결과적으로 프로덕션에서 사용할 수 있는 코드가 생성됩니다.

<br>

### Configure AWS credentials
```yaml
  - name: Configure AWS credentials
    uses: aws-actions/configure-aws-credentials@v1
    with:
      aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
      aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
      aws-region: ${{ secrets.AWS_REGION }}
```
AWS 액션 configure-aws-credentials를 사용하여 AWS 자격 증명을 구성합니다. 이를테면, AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_REGION 등을 GitHub secrets에서 가져와 설정합니다.

<br>

### Deploy to S3
```yaml
  - name: Deploy to S3
    run: |
      aws s3 sync out/ s3://${{ secrets.S3_BUCKET_NAME }} --delete
```
이 단계에서는 AWS CLI를 사용하여 빌드 결과물을 AWS S3 버킷에 동기화합니다. --delete 플래그는 S3 버킷에서 더 이상 필요하지 않은 파일을 삭제하도록 지시합니다.

<br>

### Invalidate CloudFront cache
```yaml
  - name: Invalidate CloudFront cache
    run: |
      aws cloudfront create-invalidation --distribution-id ${{ secrets.CLOUDFRONT_DISTRIBUTION_ID }} --paths "/*"
```
CloudFront의 캐시를 무효화합니다. 이것은 캐시된 콘텐츠가 최신 버전의 웹사이트를 반영하지 않을 때 유용합니다. 분포 ID와 무효화할 경로는 GitHub secrets에서 가져옵니다.

<br>

## 주요 링크
- S3 버킷 웹사이트 엔드포인트: http://hanghea.s3-website.us-east-2.amazonaws.com
- CloudFrount 배포 도메인 이름: https://dykx6yakigdlf.cloudfront.net

<br>

## Repository secret과 환경변수
- AWS_ACCESS_KEY_ID
- AWS_REGION
- AWS_SECRET_ACCESS_KEY
- CLOUDFRONT_DISTRIBUTION_ID
- S3_BUCKET_NAME
