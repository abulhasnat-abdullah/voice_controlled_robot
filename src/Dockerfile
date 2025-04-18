# Use ROS 2 Humble as base image
FROM ros:humble

# Install system dependencies
RUN apt-get update && apt-get install -y \
    python3-pip \
    python3-colcon-common-extensions \
    ros-humble-rclpy \
    ros-humble-std-msgs \
    ros-humble-geometry-msgs \
    python3-pyaudio \
    bash && \
    rm -rf /var/lib/apt/lists/*  

# Install Python dependencies (including speech recognition and sounddevice)
RUN python3 -m pip install --upgrade pip
RUN pip3 install --no-cache-dir SpeechRecognition sounddevice

# Set up the workspace
WORKDIR /ros2_ws/src

# Copy the package source code

COPY . /ros2_ws/src/

# Install other Python dependencies (EXCEPT rclpy)
COPY requirements.txt /ros2_ws/src/requirements.txt
RUN if [ -f requirements.txt ]; then grep -v "rclpy" requirements.txt > temp_requirements.txt && \
    pip3 install --no-cache-dir -r temp_requirements.txt; fi
# Ensure ROS 2 environment is sourced
SHELL ["/bin/bash", "-c"]

# Build the ROS 2 package
WORKDIR /ros2_ws
RUN source /opt/ros/humble/setup.bash && colcon build --symlink-install

# Source the environment when container starts
RUN echo "source /opt/ros/humble/setup.bash" >> /root/.bashrc
RUN echo "source /ros2_ws/install/setup.bash" >> /root/.bashrc

# Default command when container starts
CMD ["/bin/bash"]
