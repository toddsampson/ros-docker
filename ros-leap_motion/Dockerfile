FROM ros:indigo-ros-core
MAINTAINER Todd Sampson "toddsampson+docker@gmail.com"

RUN sudo apt-get -y update
RUN sudo apt-get -y install ros-indigo-leap-motion bc libgl1-mesa-glx-lts-trusty libglu1-mesa libxi6 libxxf86vm1 libgl1-mesa-glx libx11-6 libxext6 libx11-xcb1 libxcb-dri2-0 libxcb-dri3-0 libxcb-glx0 libxcb-present0 libxcb-sync1 libxcb1 libxdamage1 libxfixes3 libxshmfence1 libgl1-mesa-dri libxcb1 libx11-data libdrm-intel1 libdrm-nouveau2 libdrm-radeon1 libelf1 libllvm3.4 libtxc-dxtn-s2tc0 libglapi-mesa libxau6 libxdmcp6
#wget patch fakeroot

COPY Leap_Motion_SDK_Linux_2.3.1.tgz /tmp/Leap_Motion_SDK_Linux_2.3.1.tgz

COPY prl_mod.tar.gz /tmp/prl_mod.tar.gz

RUN sudo chmod -R 777 /tmp

RUN cd /tmp && tar -xzvf Leap_Motion_SDK_Linux_2.3.1.tgz && \
    cd LeapD* && \
    sudo dpkg --install Leap-*-x64.deb

# IMPORTANT: Update these values to your primary computer user id and group id
# On OSX use `id -u` and `id -g` to find the correct values
ENV uid 1000
ENV gid 1000

RUN export uid=${uid} gid=${gid} && \
    mkdir -p /home/ros && \
    echo "ros:x:${uid}:${gid}:ros,,,:/home/ros:/bin/bash" >> /etc/passwd && \
    echo "ros:x:${uid}:" >> /etc/group && \
    echo "ros ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/ros && \
    chmod 0440 /etc/sudoers.d/ros && \
    chown ${uid}:${gid} -R /home/ros

USER ros
ENV HOME /home/ros

RUN mkdir -p ~/.Leap\ Motion && printf '{\n  "configuration": {\n    "images_mode": 2,\n    "low_resource_mode_enabled": true\n  }\n}\n' > ~/.Leap\ Motion/config.json

RUN sudo apt-get -y install linux-headers-$(uname -r) mesa-utils xarclock lshw && cd /tmp && \
    tar xzvf prl_mod.tar.gz
RUN cd /tmp && /usr/bin/make -f Makefile.kmods
RUN sudo mkdir -p /lib/modules/$(uname -r)/parallels && \
    sudo cp /tmp/prl_tg/Toolgate/Guest/Linux/prl_tg/prl_tg.ko /lib/modules/$(uname -r)/parallels && \
    sudo depmod -A
COPY prltoolsd /etc/init.d/prltoolsd
RUN sudo chmod +x /etc/init.d/prltoolsd
RUN sudo initctl reload-configuration
