# ROS 2 Jazzy development/runtime image for teleop_twist_keyboard.
FROM ros:jazzy

SHELL ["/bin/bash", "-c"]

ENV WS=/ros2_ws
WORKDIR ${WS}

# Copy only package.xml first so dependency installation is cached
# independently of source changes.
COPY package.xml ${WS}/src/teleop_twist_keyboard/package.xml

RUN apt-get update \
    && rosdep update --rosdistro "${ROS_DISTRO}" \
    && rosdep install --from-paths src --ignore-src -y --rosdistro "${ROS_DISTRO}" \
    && rm -rf /var/lib/apt/lists/*

COPY . ${WS}/src/teleop_twist_keyboard

RUN source /opt/ros/${ROS_DISTRO}/setup.bash \
    && colcon build --symlink-install

COPY docker/entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
CMD ["ros2", "run", "teleop_twist_keyboard", "teleop_twist_keyboard"]
