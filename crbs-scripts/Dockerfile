FROM archlinux:latest
RUN pacman -Syu --noconfirm base-devel git mariadb python python-pip
RUN pip install xsdata[cli] mariadb --break-system-packages
ADD https://api.github.com/repos/horvatgroup/classroombookings_scripts/git/refs/heads/main version.json
RUN cd /opt && git clone https://github.com/horvatgroup/classroombookings_scripts
COPY ../../ssc_credentials/classroombookings_scripts/credentials.py
COPY ../../ssc_credentials/classroombookings_scripts/asctt2012.xml
WORKDIR /opt/classroombookings_scripts/scripts
CMD bash
