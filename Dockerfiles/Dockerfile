FROM buildpack-deps:zesty

WORKDIR /root
RUN wget https://github.com/cms-dev/cms/releases/download/v1.3.rc0/v1.3.rc0.tar.gz
RUN tar zxvf v1.3.rc0.tar.gz
RUN rm v1.3.rc0.tar.gz

WORKDIR /root/cms

# 相依套件
RUN apt-get update
RUN apt-get install -y \
    build-essential \
    openjdk-8-jre \
    openjdk-8-jdk \
    postgresql-client \
    gettext \
    iso-codes \
    shared-mime-info \
    stl-manual \
    cgroup-lite
RUN apt-get install -y gettext python-dev libpq-dev libcups2-dev libyaml-dev libffi-dev python-pip

# 使用者以及群組
RUN useradd cmsuser -M -r -s /bin/false -U
RUN usermod -a -G cmsuser root
RUN su - $USER

# prerequisite.py
RUN ./prerequisites.py build --as-root
RUN ./prerequisites.py install_isolate --as-root
RUN ./prerequisites.py install_conf --as-root

# 資料夾
RUN mkdir -m 755 /var/local/log
RUN mkdir -m 755 /var/local/cache
RUN mkdir -m 755 /var/local/lib
RUN mkdir -m 755 /var/local/run
RUN mkdir -m 770 /var/local/log/cms
RUN mkdir -m 770 /var/local/cache/cms
RUN mkdir -m 770 /var/local/lib/cms
RUN mkdir -m 770 /var/local/run/cms
RUN mkdir -m 770 /usr/local/include/cms
RUN mkdir -m 770 /usr/local/share/cms
RUN chown cmsuser -R /var/local/log/cms
RUN chown cmsuser -R /var/local/cache/cms
RUN chown cmsuser -R /var/local/lib/cms
RUN chown cmsuser -R /var/local/run/cms
RUN chown cmsuser -R /usr/local/include/cms
RUN chown cmsuser -R /usr/local/share/cms

# 安裝
RUN pip2 install -r requirements.txt
RUN python2 setup.py install

# 設定檔案
COPY config/cms.conf /usr/local/etc

# 執行東西
CMD cmsLogService 0 & cmsResourceService -a 1
