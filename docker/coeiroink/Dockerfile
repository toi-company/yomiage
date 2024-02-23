FROM python:3.11-slim-buster
RUN apt-get update && apt-get install -y git wget curl build-essential cmake
RUN mkdir coeiroink_build
WORKDIR /coeiroink_build
RUN mkdir speaker_info
RUN wget https://jaist.dl.sourceforge.net/project/open-jtalk/Dictionary/open_jtalk_dic-1.11/open_jtalk_dic_utf_8-1.11.tar.gz -P coeiroink_build
RUN tar -zxvf coeiroink_build/open_jtalk_dic_utf_8-1.11.tar.gz
RUN git clone https://github.com/shirowanisan/coeiroink_core
WORKDIR /coeiroink_build/coeiroink_core
RUN rm -f requirements.txt
COPY requirements.txt .
RUN --mount=type=cache,target=/root/.cache/pip pip install pip setuptools wheel --upgrade
RUN --mount=type=cache,target=/root/.cache/pip pip install git+https://github.com/espnet/espnet
RUN --mount=type=cache,target=/root/.cache/pip pip install -r requirements.txt
RUN --mount=type=cache,target=/root/.cache/pip pip install .
WORKDIR /coeiroink_build
RUN cp ./open_jtalk_dic_utf_8-1.11/* /usr/local/lib/python3.11/site-packages
RUN git clone https://github.com/toi-company/voicevox_engine
WORKDIR /coeiroink_build/voicevox_engine
RUN --mount=type=cache,target=/root/.cache/pip pip install -r requirements.txt
RUN cp -rf ../speaker_info .
COPY assets/ /coeiroink_build/assets/
RUN cp -rf /coeiroink_build/assets/* /coeiroink_build/voicevox_engine/

CMD ["python", "run.py"]