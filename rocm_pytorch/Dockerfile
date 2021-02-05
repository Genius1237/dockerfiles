FROM rocm/pytorch:rocm4.0.1_ubuntu18.04_py3.6_pytorch_1.7.0

RUN pip install -U pip

RUN mv -r /root/.local/* /opt/conda/

RUN echo 'ADD_EXTRA_GROUPS=1' | sudo tee -a /etc/adduser.conf \
    && echo 'EXTRA_GROUPS=video' | sudo tee -a /etc/adduser.conf

