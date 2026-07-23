
작성자 이찬울

1. 평소에 라이브러리와 파이썬 버전을 Colab에 맞춰서 사용한다. uv 가상환경을 이용하는 것을 기반으로 하여, pyproject.toml 파일에 라이브러리 버전을 명시한다. 이 ==라이브러리 버전을 Colab에 맞춰 사용하다가, GCP VM 사용제한이 걸리는 사고가 발생할 경우 Google Colab의 GPU를 활용한다.== Google Colab은 GCP VM 보다 제약사항이 많아 쉘과 터미널의 파이썬 라이브러리 버전을 사용자가 자유자제로 변경하여 사용할 수 없다는 점에 착안하여, ==평소에 Google Colab의 라이브러리 버전에 맞춰 GCP VM을 사용한다.== GCP VM 사용이 제한되어 팀 프로젝트가 개발이 멈추는 상황에 대한 Plan B로 Google Colab을 염두해둔다.
