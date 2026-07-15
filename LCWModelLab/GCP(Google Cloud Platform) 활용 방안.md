
작성자 이찬울


1. OS : Linux CLI(CommandLine Interface) 환경

2. 깃허브 클론 & Pull : 깃허브 레포지토리를 VM 개인 소유자 권한의 경로에 클론 또는 풀하여 AI 프로그램 개발을 진행한다.

  <개인 소유자 권한의 경로 확인>
  홈 디렉토리로 이동 : $ cd ~
  현재 작업 디렉토리 확인 : $ pwd

3. 클론이나 풀하여 로드한 프로젝트를 실행하는 방법 : uv 가상환경을 활용하여 .toml 파일에 작성되어 있는 라이브러리 및 파이썬 버전을 맞춘다.
  <uv 설치 및 세팅은 concepts/# uv 설정 가이드 참고>

4. 프로젝트 구동에 필요한 데이터셋을 이동시키는 방법

  <우선적으로 concepts/# VS Code ↔ GCP VM 원격 개발 환경 연결 가이드를 참고>
  
  <사용자 로컬 환경에서 GCP CLI Linux 환경으로 데이터셋 이동하는 방법>
  gcloud 설치 진행 : https://docs.cloud.google.com/sdk/docs/install-sdk?hl=ko
  
  ㄱ. 로컬의 파일 하나를 GCP로 보낼 때
	gcloud compute scp /로컬/파일/경로 [VM인스턴스이름]:/home/ChanwoolLee/ --zone=[존이름]

  ㄴ. 로컬의 폴더 전체를 통째로 보낼 때 (-r 옵션 추가)
	gcloud compute scp --recurse /로컬/폴더/경로 [VM인스턴스이름]:/home/ChanwoolLee/ --zone=[존이름]

 <VM 인스턴스 이름과 zone 이름 확인법>
	 gcloud compute instances list
	 ->입력하면 다음과 같이 보입니다.
	 NAME               ZONE               MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP    STATUS
	hongildong     asia-east3-pa          ou1-stand-4                            10.142.9.2   25.63.73.123      RUNNING

	 이 중 인스턴스이름은 NAME 항목에, 존이름은 ZONE에 있습니다.

	* 데이터는 사용자가 GCP의 원하는 경로에 이동시켜서 프로젝트를 구동할 때 활용합다. *

5. GCP 사용의 문제점 : 2팀의 팀원 5명이 하나의 VM 환경을 공유함. 용량 100GB 제한.

  용량의 대부분을 차지하는 Datasets, Visulization, Images, 등을 필요해서 무분별 copy, 깃허브 레포지토리를 정리하지 않고 여러 개 남겨두거나, 가상환경을 사용하지 않고 라이브러리를 직접 설치, 파이썬 프로그램을 직접 설치하는 경우 등 누적되면 VM이 한계 사용량을 넘을 수 있기 때문에,

 ㄱ. VM을 사용하지 못하는 경우가 발생하면, GPU를 사용해야 하므로 Colab 환경을 이용하기 위해 미리 Colab과 파이썬 및 라이브러리 버전을 맞추며 사용한다. 깃허브 프로젝트의 uv 명령어에 활용되는 .toml 파일을 Colab 환경의 버전들에 맞춰서 사용한다.

 ㄴ. GCS를 이용하여 GCP에서 생성되는 데이터를 실시간으로 공유할 수 있는 환경을 구성한다. VM이 한계 사용량을 넘어서서 사용하게 되지 못할 때 데이터가 보존될 수 있다.

 ㄷ. GCP에서 필요없는 폴더와 파일들은 정리하면서 사용한다.

< 폴더 및 하위 모든 파일들을 삭제하는 명령어 >

cd로 작업디렉토리 이동 후, rm -rf [폴더 이름] 또는 rm -rf [폴더의 절대 경로]