^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Changelog for package hector_gazebo_plugins
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

0.5.2 (2020-12-17)
------------------
* Add case for rotating in place in odometry (`#75 <https://github.com/tu-darmstadt-ros-pkg/hector_gazebo/issues/75>`_)
  * Add case for rotating in place in odometry
  Odometry calculations were not taking in account the case of the robot rotating in place. This
  would cause a division by zero and make the plugin return invalid data. This commit handles this
  case by not using the arc method if the distance traveled by the robot is close to zero.
  * Fix negative Y-axis motion bug
  Only the displacement along X and the combined displacement were used to
  calculate the angle between the X-axis and the robot drive direction.
  This resulted in a positive angle whatever if the displacement along Y
  was positive or negative. This commit addresses the issue by using both
  displacements along X and Y to calculate the angle.
* Allow publishing sensor_msgs/MagneticField (`#73 <https://github.com/tu-darmstadt-ros-pkg/hector_gazebo/issues/73>`_)
  Add a new useMagneticFieldMsgs parameter to the magnetometer plugin, allowing the node to publish sensor_msgs/MagneticField messages instead of std_msgs/Vector3Stamped
* Add support of holonomic robot by adding missing Y component in odometry (`#71 <https://github.com/tu-darmstadt-ros-pkg/hector_gazebo/issues/71>`_)
  * Add support of holonomic robot by adding missing Y component in odometry
  The odometry was only computed taking in account linear velocity on the x-axis and angular
  velocity around the z-axis. The code is modified to include linear velocity on the y-axis for
  support of holonomic robots. Odometry arc and radius are calculated the same way as before but
  using the combined linear velocity instead. The end position is then rotated by the angle between
  the combined velocity vector and the x-axis
  * Change variable name
  * Rename combined velocity angle variable
* Merge pull request `#64 <https://github.com/tu-darmstadt-ros-pkg/hector_gazebo/issues/64>`_ from ms-iot/upstream_windows_fix
  [Windows][melodic-devel] Follow catkin guide to update the install path
* fix install path. (`#1 <https://github.com/tu-darmstadt-ros-pkg/hector_gazebo/issues/1>`_)
* Merge pull request `#53 <https://github.com/tu-darmstadt-ros-pkg/hector_gazebo/issues/53>`_ from enwaytech/ml/cmp0054_fix
  add CMP0045 cmake policy before gazebo include in CMakeLists.txt
* add CMP0045 cmake policy before gazebo include in CMakeLists.txt
* Contributors: Chris I-B, Johannes Meyer, Matthias Loebach, RobinB, Sean Yen, Stefan Fabian

0.5.1 (2018-06-29)
------------------
* Merge pull request `#44 <https://github.com/tu-darmstadt-ros-pkg/hector_gazebo/issues/44>`_ from esteve/gazebo8
  Use Gazebo 8 APIs to avoid deprecation warnings.
* Fix indent
* Depend on gazebo_dev instead
* Use sdf::Time to read time periods from SDF files.
* Only include math headers when building for Gazebo < 8
* Use Gazebo 8 APIs to avoid deprecation warnings.
* Merge pull request `#36 <https://github.com/tu-darmstadt-ros-pkg/hector_gazebo/issues/36>`_ from vincentrou/get_fix_status
  [gazebo_plugins] Ensure to get gps fix status as an int in urdf
* hector_gazebo_plugins: numeric conversion of NavSatFix status and service from SDF without std::stoi
* [gazebo_plugins] Ensure to get gps fix status as an int
* hector_gazebo_plugins/hector_gazebo_thermal_camera: removed catkin_package(DEPENDS gazebo) declaration which was a no-op anyway
  See https://github.com/ros-simulation/gazebo_ros_pkgs/pull/537.
* Contributors: Esteve Fernandez, Johannes Meyer, Vincent Rousseau

0.5.0 (2016-06-24)
------------------
* Updated gazebo dependencies to version 7 for kinetic release
* Contributors: Johannes Meyer

0.4.1 (2016-06-24)
------------------
* see 0.3.8

0.4.0 (2015-11-07)
------------------
* Added proper dependencies for jade and gazebo5. Now compiles and works for gazebo5
* Contributors: L0g1x

0.3.8 (2016-06-24)
------------------
* hector_gazebo_plugins: fixed angular rate output with non-zero rpyOffset parameter (fix #23)
* Fix compilation errors and warnings against Gazebo 7
* Fix
* Compatible with gazebo7
* [hector_gazebo_plugins] fix missing dependencies
* Update odometry with proper covariance data
* Updates to 2d odom
* Basics of 2d odom
* Add gitignore
* Add Cmake flags for C++11
* Make force based p gains parameters
* Contributors: Furushchev, Johannes Meyer, Nate Koenig, Nicolae Rosia, Romain Reignier, Stefan Kohlbrecher

0.3.7 (2015-11-07)
------------------
* gazebo_ros_force_based_move: Disable odom tf publishing if set in sdf params
* gazebo_ros_force_based_move: Add plugin for applying forces based on cmd_vel input (allows simulation of tracked vehicles)
* hector_gazebo_plugins/hector_gazebo_thermal_camera: switch to cmake configuration for gazebo and added OGRE include directories required for CameraPlugin.hh
* Contributors: Johannes Meyer, kohlbrecher

0.3.6 (2015-03-21)
------------------
* allow negative offsets in SensorModel dynamic_reconfigure config
* fixed sporadic NaN values in gazebo_ros_imu's angular_velocity output (fix #20)
* reintroduced orientation bias due to accelerometer and yaw sensor drift
  This orientation bias was removed in 74b21b7, but there is no need for this.
  The IMU can have a mounting offset and a bias error. To remove the bias error, set the accelerationDrift and yawDrift parameters to 0.
* interpret parameters xyzOffset and rpyOffset as pure mounting offsets and not as induced by accelerometer bias (fix #18)
  Obviously the SDF conversion assumes that all sensor plugins interpret the `<xyzOffset>` and `<rpyOffset>` parameters in the same way as an
  additional sensor link which is connected with a static joint to the real parent frame. I was not aware that this is a requirement.
  hector_gazebo_ros_imu interpreted the roll and pitch part of `<rpyOffset>` as an orientation offset caused by a (small) accelerometer bias.
  This patch completely removes the bias error on the orientation output in the Imu message and the orientation quaternion in the bias message
  is set to zero.
* Contributors: Johannes Meyer

0.3.5 (2015-02-23)
------------------
* fixed simulated IMU calibration
* fill position_covariance in sensor_msgs/NavSatFix message from position error model in gazebo_ros_gps (fix #17)
* added dynamic_reconfigure servers to gps, magneto and sonar plugins
  The GPS plugin allows to configure the status and server flags in the output message,
  additionally to the error characteristics. This allows to simulate GPS dropouts.
* added dynamic_reconfigure server for IMU sensor model parameters
* fixed invocation of sensor model in gazebo_ros_imu and gazebo_ros_sonar to also respect the scale error
* calculate angular rate from quaternion difference directly
  This seems to be numerically more stable and removes the jitter in the angular rate signal.
* added initial bias
* added bias publisher to gazebo_ros_imu
  ...to compare hector_pose_estimation estimates with ground truth.
  Also renamed heading to yaw in gazebo_ros_imu and updated pseudo AHRS orientation calculation.
* added scale error to the sensor model and removed linearization in drift update
  The scale error is assumed to be constant and its value is loaded from the `scaleError` plugin parameter.
  The default scale error is 1.0 (no scale error).
  The value returned by the model is `(real_value * scale_error) + offset + drift + noise`.
* fixed wrong calculation of reference earth magnetic field vector if declination!=0
* Contributors: Johannes Meyer

0.3.4 (2014-09-01)
------------------
* replaced old copied license header in servo_plugin.cpp
* simplified attitude error calculation in gazebo_ros_imu (fixes #12)
* fixed calculation of vector-valued sensor errors and sensor error model resetting with non-zero initial drift
* linking against catkin_libraries
* Contributors: Johannes Meyer, Markus Achtelik

0.3.3 (2014-05-27)
------------------

0.3.2 (2014-03-30)
------------------
* diffdrive_plugin_multi_wheel: Fix wrong whell rotation calculation (Was only half speed of desired)
* diffdrive_plugin_multi_wheel: Optionally do not publish odometry via tf or as msg
* Fixed boost 1.53 issues
  Replaced boost::shared_dynamic_cast with boost::dynamic_pointer_cast
* Add servo plugin (used for vision box currently)
* Add catkin_LIBRARIES to linking for multiwheel plugin
* Some fixes to make diffdrive_plugin_multi_wheel work properly
* Work in progress of a diffdrive plugin supporting multiple wheels per side
* used updated API to get rid of warnings
* added topicName parameter back to gazebo_ros_magnetic
* hector_gazebo: deleted deprecated export sections from package.xml files
* abort with a fatal error if ROS is not yet initialized + minor code cleanup
* Contributors: Christopher Hrabia, Johannes Meyer, Richard Williams, Stefan Kohlbrecher, richardw347

0.3.1 (2013-09-23)
------------------
* fixed a bug in UpdateTimer class when updateRate and updatePeriod parameters are unset
* removed warnings due to deprecated sdf API calls

0.3.0 (2013-09-02)
------------------
* Catkinization of stack hector_gazebo
