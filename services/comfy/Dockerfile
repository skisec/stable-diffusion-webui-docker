FROM pytorch/pytorch:2.5.1-cuda12.4-cudnn9-runtime

ENV DEBIAN_FRONTEND=noninteractive PIP_PREFER_BINARY=1

RUN apt-get update && apt-get install -y git && apt-get clean

ENV ROOT=/stable-diffusion
RUN --mount=type=cache,target=/root/.cache/pip \
  git clone https://github.com/comfyanonymous/ComfyUI.git ${ROOT} && \
  cd ${ROOT} && \
  git checkout master && \
  git reset --hard 276f8fce9f5a80b500947fb5745a4dde9e84622d && \
  pip install -r requirements.txt

RUN git clone https://github.com/ltdrdata/ComfyUI-Manager ${ROOT}/custom_nodes/ComfyUI-Manager && \
pip install -r ${ROOT}/custom_nodes/ComfyUI-Manager/requirements.txt

RUN git clone https://github.com/Efficient-Large-Model/ComfyUI_ExtraModels ${ROOT}/custom_nodes/ComfyUI_ExtraModels && \
pip install -r ${ROOT}/custom_nodes/ComfyUI_ExtraModels/requirements.txt

WORKDIR ${ROOT}
COPY . /docker/
RUN chmod u+x /docker/entrypoint.sh && cp /docker/extra_model_paths.yaml ${ROOT}

ENV NVIDIA_VISIBLE_DEVICES=all PYTHONPATH="${PYTHONPATH}:${PWD}" CLI_ARGS=""
EXPOSE 7860
ENTRYPOINT ["/docker/entrypoint.sh"]
CMD python -u main.py --listen --port 7860 ${CLI_ARGS}
