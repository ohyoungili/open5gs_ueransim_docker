# open5gs_ueransim_docker

1. 도커 이미지 빌드

open5gs 에서 
docker build -f Dockerfile_upf1 --tag open5gs_upf1:2.3.0 .
docker build -f Dockerfile_upf2 --tag open5gs_upf2:2.3.0 .
docker build -f Dockerfile.webui --tag open5gs-webui:2.3.0 .

ueransim 에서
docker build --tag ueransim:3.2.2 .


2. 도커 컨테이너 관리 툴 portainer 실행

도커 컨테이너들을 쉽게 확인하고 변경 할 수 있는 툴입니다.
이 것 사용 안하고 하나 씩 직접 CLI 로 해도 되지만 굳이...

docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data --restart=always portainer/portainer

자세한 내용은 아래 링크 참조
https://help.iwinv.kr/manual/read.html?idx=548


3. open5gs 실행 하기

NF 들을 docker-compose 로 실행 합니다.

docker-compose -f ngc.yaml up -d

아래 스크립트로 몽고 DB 에 가입자 정보를 업데이트 합니다.
./register_subscribers_multi_dnn.sh


3. ueransim 실행 하기

gnb 2개, ue 4개를 실행 합니다.

docker-compose -f 2gnb-4ue.yaml up -d

gnb 와 ue 는 open5gs 의 subnet 에 포함 되지 않은 상태이기 때문에 정상 동작 하지 않습니다.


4. portainer 로 gnb1->gnb2->ue1->ue2->ue3->ue4 컨테이너를 open5gs subnet 에 추가 하여 실행 하기

 - gnb1을 open5gs subnet 에 추가 하기 
   open5gsnetwork 를 선택하여 옆의 join network 버튼을 누릅니다.
    네트워크 목록에 기존 default 와 2개가 있는 데 기존 default 는 leave network 버튼을 눌러 삭제 합니다.
   gnb는 stopped 된 상태로 start 버튼을 선택합니다.
   
 - gnb1->gnb2->ue1->ue2->ue3->ue4 순서로 동일하게 처리 하면 됩니다.
    참고로 ue 는 runnig 상태 일 경우 start 버튼이 아니라 restart 버튼을 선택 해야 합니다.


