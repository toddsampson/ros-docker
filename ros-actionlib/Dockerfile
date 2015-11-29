FROM ros:indigo-ros-core
MAINTAINER Todd Sampson "toddsampson+docker@gmail.com"

RUN apt-get -y update && \
    apt-get -y install ros-indigo-actionlib ros-indigo-actionlib-msgs ros-indigo-actionlib-tutorials

RUN mkdir -p /opt/ros/indigo/share/actionlib_tutorials/simple_action_servers && \
    mkdir -p /opt/ros/indigo/share/actionlib_tutorials/simple_action_client

COPY ./fibonacci_client.py /opt/ros/indigo/share/actionlib_tutorials/simple_action_client/fibonacci_client.py
COPY ./fibonacci_server.py /opt/ros/indigo/share/actionlib_tutorials/simple_action_servers/fibonacci_server.py

RUN chmod +x /opt/ros/indigo/share/actionlib_tutorials/simple_action_client/fibonacci_client.py && \
    chmod +x /opt/ros/indigo/share/actionlib_tutorials/simple_action_servers/fibonacci_server.py

CMD bash
