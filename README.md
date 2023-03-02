# ROS Sample 

This sample shows how to build up a ros workspace with multiple dependencies. This includes own non-ros dependencies which are not in the ros index.


Build:
```bash
colcon build --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DCPM_SOURCE_CACHE="../../.cache/cpm" --event-handlers console_cohesion+ --packages-up-to super
```