---
cover: ../.gitbook/assets/Frame 81.png
coverY: 0
---

# Github과 AWS를 연동하여 CI/CD 구축

GitHub Actions와 AWS IAM, AWS S3, AWS EC2, AWS CodeDeploy를 활용하여 배포 CI/CD 구축하는 방법입니다.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>IAM 서비스에서 사용자로 이동 후 사용자 생성</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>사용자 이름 입력 후 다음</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p>직접 정책 연결 선택 후 다음 3 가지 정책 선택하고 다음</p></figcaption></figure>

* 선택해야 하는 권한 정책
  * AmazonEC2FullAccess
  * AmazonS3FullAccess
  * AWSCodeDeployFullAccess



<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption><p>검토 후 사용자 생성</p></figcaption></figure>

<figure><img src="../.gitbook/assets/스크린샷 2024-10-23 오후 3.10.56.png" alt=""><figcaption><p>생성된 IAM 확인 후 액세스 키 만들기 선택</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption><p>기타 선택 후 다음으로 이동</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption><p>설명 태그 값 입력 후 액세스 키 만들기 선택</p></figcaption></figure>

<figure><img src="../.gitbook/assets/스크린샷 2024-10-23 오후 3.19.47.png" alt=""><figcaption><p>액세스 키와 비밀 액세스 키를 따로 기록하거나 csv 파일 다운로드 후 완료 선택</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>GitHub Repository에 Secrets 등록 페이지로 이동 후 키 등록</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption><p>액세스 키 ID와 비밀 액세스 키 등록</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption><p>AWS S3로 이동 후 버킷 만들기 선택</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption><p>버킷 이름만 설정한 후 버킷 만들기 선택</p></figcaption></figure>

