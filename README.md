# point_lio_unilidar

## 1. Wprowadzenie

### 1.1 Unitree LiDAR

Niniejsze repozytorium dostosowuje najnowocześniejszy algorytm lidarowej odometrii inercyjnej `Point-LIO` do użytku z naszymi produktami lidarowymi:
- `Unitree LiDAR L1`
- `Unitree LiDAR L2`

Zarówno `L1`, jak i `L2` posiadają następujące cechy:
- duże pole widzenia (360° × 90°)
- skanowanie niepowtarzające się
- niski koszt
- odpowiedniość do zastosowań w niskopręd kościowych robotach mobilnych

Więcej informacji o naszych produktach lidarowych można znaleźć na oficjalnej stronie internetowej.
- <https://www.unitree.com/L2>
- <https://www.unitree.com/LiDAR>


### 1.2 Point-LIO

`Point-LIO` to odporny i szerokopasmowy lidarowy system odometrii inercyjnej (LIO), zdolny do zapewnienia dokładnej, wysokoczęstotliwościowej odometrii oraz niezawodnego mapowania nawet przy silnych wibracjach i gwałtownych ruchach. Więcej informacji o algorytmie `Point-LIO` można znaleźć na oficjalnej stronie oraz w publikacji naukowej:
- <https://github.com/hku-mars/Point-LIO>
- [Point‐LIO: Robust High‐Bandwidth Light Detection and Ranging Inertial Odometry](https://onlinelibrary.wiley.com/doi/epdf/10.1002/aisy.202200459)


## 2. Wideo demonstracyjne

### 2.1 L1 LiDAR

[![Video](./doc/video.png)](https://oss-global-cdn.unitree.com/static/c0bd0ac7d1e147e7a7eaf909f1fc214f.mp4 "SLAM based on Unitree 4D LiDAR L1")

### 2.2 L2 LiDAR
 
[![Video](./doc/l2-demo-video-bilibili.png)](https://www.bilibili.com/video/BV1XVUVYHEHR "SLAM based on Unitree 4D LiDAR L2")

[YouTube](https://youtu.be/juAfGrg2xBg?si=IVTWM9shEmHsKKJ_)


## 3. Wymagania wstępne

### 3.1 Ubuntu i ROS
Kod był testowany na Ubuntu 20.04 z [ROS noetic](http://wiki.ros.org/noetic/Installation/Ubuntu). Ubuntu 18.04 i starsze wersje mają problemy ze środowiskiem niezbędnym do uruchomienia Point-LIO — zaleca się unikanie korzystania z Point-LIO na tych systemach.

Instalację ROS noetic można przeprowadzić zgodnie z oficjalną dokumentacją:
- <http://wiki.ros.org/noetic/Installation/Ubuntu>

Wymagany jest dodatkowy pakiet ROS:
```
sudo apt-get install ros-xxx-pcl-conversions
```

### 3.2 Eigen
Postępuj zgodnie z oficjalną [instrukcją instalacji Eigen](eigen.tuxfamily.org/index.php?title=Main_Page) lub zainstaluj Eigen bezpośrednio poleceniem:
```
sudo apt-get install libeigen3-dev
```

### 3.3 unilidar_sdk

Aby korzystać z lidaru `L1`, należy pobrać i zbudować [unilidar_sdk](https://github.com/unitreerobotics/unilidar_sdk) wykonując poniższe kroki:

```
git clone https://github.com/unitreerobotics/unilidar_sdk.git

cd unilidar_sdk/unitree_lidar_ros

catkin_make
```

### 3.4 unilidar_sdk2

Aby korzystać z lidaru `L2`, należy pobrać i zbudować [unilidar_sdk2](https://github.com/unitreerobotics/unilidar_sdk2) wykonując poniższe kroki:

```
git clone https://github.com/unitreerobotics/unilidar_sdk2.git

cd unilidar_sdk/unitree_lidar_ros

catkin_make
```

## 4. Kompilacja

Sklonuj to repozytorium i uruchom `catkin_make`:

```
mkdir -p catkin_point_lio_unilidar/src

cd catkin_point_lio_unilidar/src

git clone https://github.com/unitreerobotics/point_lio_unilidar.git

cd ..

catkin_make
```


## 5. Uruchamianie

### 5.1 Uruchamianie z L1

Aby zapewnić prawidłową inicjalizację IMU, zaleca się utrzymywanie lidaru w stanie nieruchomym przez pierwsze kilka sekund działania algorytmu.

Uruchom `unilidar`:
```
cd unilidar_sdk/unitree_lidar_ros

source devel/setup.bash

roslaunch unitree_lidar_ros run_without_rviz.launch
```

Uruchom `Point-LIO`:
```
cd catkin_unilidar_point_lio

source devel/setup.bash

roslaunch point_lio_unilidar mapping_unilidar_l1.launch 
```


Po zakończeniu działania cała zbuforowana mapa chmury punktów zostanie zapisana pod następującą ścieżką:
```
catkin_point_lio_unilidar/src/point_lio_unilidar/PCD/scans.pcd
```

Do przeglądania tego pliku pcd można użyć narzędzia `pcl_viewer`:
```
pcl_viewer scans.pcd 
```

### 5.2 Uruchamianie z rosbag dla L1

Jeśli nie posiadasz jeszcze naszego lidaru, możesz pobrać zestaw danych nagrany przy użyciu naszego lidaru i przetestować na nim działanie algorytmu.
Adres pobierania:
- [unilidar-2023-09-22-12-42-04.bag - Pobierz](https://oss-global-cdn.unitree.com/static/unilidar-2023-09-22-12-42-04.zip)


Uruchom `Point-LIO`:
```
cd catkin_point_lio_unilidar

source devel/setup.bash

roslaunch point_lio_unilidar mapping_unilidar_l1.launch 
```

Odtwórz pobrany zestaw danych:
```
rosbag play unilidar-2023-09-22-12-42-04.bag 
```


Po zakończeniu działania cała zbuforowana mapa chmury punktów zostanie zapisana pod następującą ścieżką:
```
catkin_point_lio_unilidar/src/point_lio_unilidarPCD/scans.pcd
```

Do przeglądania tego pliku pcd można użyć narzędzia `pcl_viewer`:
```
pcl_viewer scans.pcd 
```

### 5.3 Uruchamianie z L2

Aby zapewnić prawidłową inicjalizację IMU, zaleca się utrzymywanie lidaru w stanie nieruchomym przez pierwsze kilka sekund działania algorytmu.

Uruchom `unilidar`:
```
cd unilidar_sdk/unitree_lidar_ros

source devel/setup.bash

roslaunch unitree_lidar_ros run_without_rviz.launch
```

Uruchom `Point-LIO`:
```
cd catkin_unilidar_point_lio

source devel/setup.bash

roslaunch point_lio_unilidar mapping_unilidar_l2.launch 
```

Po zakończeniu działania cała zbuforowana mapa chmury punktów zostanie zapisana pod następującą ścieżką:
```
catkin_point_lio_unilidar/src/point_lio_unilidar/PCD/scans.pcd
```

Do przeglądania tego pliku pcd można użyć narzędzia `pcl_viewer`:
```
pcl_viewer scans.pcd 
```

### 5.4 Uruchamianie z rosbag dla L2

Jeśli nie posiadasz jeszcze naszego lidaru, możesz pobrać zestaw danych nagrany przy użyciu naszego lidaru i przetestować na nim działanie algorytmu.
Adresy pobierania:
- [L2 Indoor Point Cloud Data.bag - Pobierz](https://oss-global-cdn.unitree.com/static/L2%20Indoor%20Point%20Cloud%20Data.bag)
- [L2 Park Observed Point Cloud Data.bag - Pobierz](https://oss-global-cdn.unitree.com/static/L2%20Park%20Point%20Cloud%20Data.bag)


Uruchom `Point-LIO`:
```
cd catkin_point_lio_unilidar

source devel/setup.bash

roslaunch point_lio_unilidar mapping_unilidar_l2.launch 
```

Odtwórz pobrany zestaw danych:
```
rosbag play XXXXXX.bag 
```

Po zakończeniu działania cała zbuforowana mapa chmury punktów zostanie zapisana pod następującą ścieżką:
```
catkin_point_lio_unilidar/src/point_lio_unilidarPCD/scans.pcd
```

Do przeglądania tego pliku pcd można użyć narzędzia `pcl_viewer`:
```
pcl_viewer scans.pcd 
```