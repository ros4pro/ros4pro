# ROS4PRO : Journée Intégration

## 1. Architecture

Le scenario est le suivant :

1. Le réseau de neurone a été entraîné au préalable sur les batchs MNIST avec `learn.py`
2. Sawyer prend une photo au dessus du feeder avec sa camera `right_hand_camera` (étape "scan")
3. Cette photo est envoyée au serveur de vision `vision_server.py` qui va :
  3.1. extraire les contours carrés des cubes
  3.2. transmettre les imagettes rognées selon leurs contours au réseau de neurones
  3.3. le réseau de neurone va effectuer une prédiction sur le label marqué à la main 1 ou 2
4. Le noeud de manipulation récupère les coordonnées des cubes et leur label
5. Pour chaque cube, il effectue un pick-and-place pour le positionner sur la remorque du Turtlebot. Le label est passé sur un paramètre `/ros4pro/label`
6. Le Turtlebot lit ce paramètre et se rend au conteneur 1 ou 2
7. Il effectue une rotation de 360° pour faire chuter le cube dans la zone de tri à l'aide du mât

### 2. Les points d'entrée 
Le paramètre `vision` indiquera au noeud de manipulation qu'il doit faire appel au serveur de vision et donc au réseau de neurones plutôt que d'utiliser l'emplacement vert prédéfini :
```
roslaunch ros4pro manipulate.launch simulate:=false vision:=true
```
