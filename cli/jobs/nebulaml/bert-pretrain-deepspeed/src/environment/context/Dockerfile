FROM ptebic.azurecr.io/public/azureml/aifx/stable-ubuntu2004-cu117-py38-torch1131:biweekly.202301.2
# RUN pip install deepspeed -U
RUN pip install git+https://github.com/huggingface/transformers
RUN pip install datasets
RUN pip install accelerate
RUN pip install scikit-learn
RUN pip install apache_beam
RUN pip install evaluate

RUN pip install git+https://github.com/FreyaRao/DeepSpeed.git@qrao/fix_save_16mit_model_error

# Install pip dependencies
RUN pip install 'ipykernel~=6.0' \
                'azureml-core' \
                'azureml-dataset-runtime' \
                'azureml-defaults' \
                'azure-ml-component' \
                'azureml-mlflow' \
                'azureml-contrib-services' \
                'azureml-contrib-services' \
                'torch-tb-profiler~=0.4.0' \
                'py-spy==0.3.12' \
                'debugpy~=1.6.3'

RUN pip install \
        azure-ai-ml \
        azureml-inference-server-http \
        inference-schema~=1.4.2.1 \
        MarkupSafe==2.0.1 \
        regex \
        pybind11

# Inference requirements
COPY --from=mcr.microsoft.com/azureml/o16n-base/python-assets:20220607.v1 /artifacts /var/
RUN /var/requirements/install_system_requirements.sh && \
    cp /var/configuration/rsyslog.conf /etc/rsyslog.conf && \
    cp /var/configuration/nginx.conf /etc/nginx/sites-available/app && \
    ln -sf /etc/nginx/sites-available/app /etc/nginx/sites-enabled/app && \
    rm -f /etc/nginx/sites-enabled/default
ENV SVDIR=/var/runit
ENV WORKER_TIMEOUT=400
EXPOSE 5001 8883 8888

# support Deepspeed launcher requirement of passwordless ssh login
RUN apt-get update
RUN apt-get install -y openssh-server openssh-client


