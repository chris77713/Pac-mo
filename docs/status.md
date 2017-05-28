---
layout: default
title: Status
---

## Project Summary: 
Project Pac-Mo is an AI agent that plays a modified version of the Pac-Man by Bandai Namco Games. The AI of Pac-Mo will be developed based on Minecraft Malmo. The goal of this game is to get the highest score from each stage, while the agent should avoid a monster in the closed map(14 by 14 for now) that kills the player at once if contacted. The dots, which gives a score in the original game, will be replaced by "the gold_ingot" in Minecraft. 

The monster in Pac-Mo, unlike the original game, cannot be eaten by the player; it is controlled by another client and its ability to chase the player is to find the shortest path from each monster to the player for each movement of the player. To give a full perspective of the map, a client of a watcher is added as well. The input for the agent will be an information of visible grid cell, such as vertically or horizontally reachable cells from current cell not blocked by walls or monsters. Then the agent will determine its best direction to obtain more "gold_ingot" and not to be killed by the monsters.


## Approach:
- Dijkstra's Algorithm:
<br>We are using Dijkstra's shortest path algorithm to calculate the monster's movement. For each step of movement, the algorithm calculates its next location from current cell in its shortest path; the algorithm moster_action returns its turn ratio relative to the monster's current degree (turn). Hence, the monster is always chasing to the player with a half of speed that the player walk.
   
- Tabular Q-learning:
<br>We are using Tabular Q-learing method for the player's (robot) movement.

## Evaluation:
- Measurement:
<br>Current evaluation process is based on the number of steps and the number of missions until the player (Robot) reaches to the solution. The term solution is not the best solution yet; in fact, finding the best solution is not trivial since the game have moving monster that is chasing after the player. __Hence, we decided to compare the number of missions until some solution for each game in the current version (1.6).__ Current version has a range of 2-14 missions to some solution; interestingly, most solutions had 163 turns until the end of the game (solution state).

- Comparison by version: 1.6 vs. 1.4
<br>The following graph represents the number of missions until the player reaches some solution:
<br>![Alt Text](https://github.com/qdingqim/Pac-mo/raw/master/docs/status_etc/graph.png)
<br> Notice that version 1.4 did not even reach to any solution during the test of more than 200 missions. However, version 1.6 was able to finish acquiring all twenty-seven 'gold_ingot' within 2-14 missions. Hence, the choose_action function revised in 1.6 has better path searching performance than the function from version 1.4

## Remaining Goals and Challenges:
- Improving Q-learing:
  
  We are trying to improve cases, where the q-values must be updated. Current version (1.6) Q-values are accumulated based on the success of each movement. For example, if the player succeed moving to the next cell chosen by the choose_action algorithm, the Q-value is updated with reward 1, in which its direction from the previous cell is updated. The player accumulates another 1 reward from picking up the "gold_ingot" in the game. The Q-values are decreased if and only if either the player detects the wall or the player is killed (contacted) to the monster. 

  Since for most cases, the Q-table are implemented in a static enviroment.(eg lava, fixed item). However for our game, the monster follows Dijkstra to the agent which means the pattern of its movement is somehow unpredictable. In this case, q-table doesn't work perfectly well. Our next goal is to find diverse cases of the q-value update instances and improve the accuracy of the Q-table or to find a better algorithm to help the agent learn.

- Make more possible paths in the map:		
  
  Current version (1.6) has one square shape map. We are going to add more paths in between maps; test out if the player can finish the game as Q-table is accumulated.
