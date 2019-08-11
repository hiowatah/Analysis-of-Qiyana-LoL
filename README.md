# Analysis of Laning in League of Legends and a deep dive on Qiyana, the latest champion to join the roster

<p align="center">
  <img src="./Images/Qiyana Image.jpg" title="Qiyana">
</p>

This project is set out to look at the types of playstyles in each role of the game League of Legends. I achieve this through looking at the match history of the best players in the North American server and then grouping the champions played in the different roles. Based on that grouping I am going to look at game statistics to further group similar playstyles by champion using K-Means Clustering. I obtain 30,000 matches to ensure I have sufficient data to conduct a proper analysis of playstyles and to obtain as many matches of Qiyana as possible. 

Qiyana has been out for One week from when I obtained my data and as such the variance in her scores is bound to be high as many different players try her out and obtain an understanding of her kit. My deep dive into Qiyana is to compare her to other champions in her cluster and to determine if she is an all around better champion based on her damage output and overall utility provided using T-Tests.

# EDA

## Obtaining the Data

To obtain my data, I used the Riot API at https://developer.riotgames.com/. 

First I obtained the list of players ranked Challenger on the North American Server. Challenger is the highest rank possible in League of Legends which means players who achieve this rank are the best players on the server.

<p align="center">
  <u><b> Challenger Player Winrates </b></u>
</p> 
<p align="center">
  <img src="./Images/Win_rates.png" title="Challenger">
</p>

The graph above shows the regression plot of the wins and losses of the players ranked Challenger when I pulled my data. As the plot indicates, these players have a greater than 50% win rate which means they win more games than they lose. This is vital since winning increases a players Match Making Rating which results in a higher rank. 

The second step, after obtaining the summoner names of the players ranked Challenger was to obtain their unique Account Ids. This was achieved through a second API call.

The third and final step was to obtain the match histories of the players. I obtained 100 match histories per player resulting in 30,000 total data points. However, as I was obtain match histories, which includes 9 other players per match, their were bound to be many duplicate entries. As such, I dropped all duplicates which resulted in a total data set of around 12,000 match histories to analyze.

## Segregating the different roles

<p align="center">
  <u><b> Challenger Lanes </b></u>
</p> 
<p align="center">
  <img src="./Images/Lanes_Challenger.png" title="Challenger">
</p>

Trying to group the champions played in each role proved to be a very difficult step during the EDA process. Because there are 300 players in Challenger and based on the graph above, a majority of them are Bottom or Support, a player will be "auto-filled" into a role other then their primary role. They will then "role swap" with another player in order to obtain their primary role. For example, if someone queued up for a game hoping to play Middle or Jungle, but the game determined their role to be Bottom, then they would try to trade their role to play Middle or Jungle but the game data will still determine them to be "Bottom". As such, the graph above and the overall data can be very misleading if looked through the lens of lane assignment. 

In order to obtain an accurate picture of the current state of the game, the analysis should be determined on the common champions played in each role and manually segregate champions into different roles and perform an analysis that way. This is not the ideal situation as champions like Irelia are commonly played in the top lane and mid lane. However, for clustering purposes, the best approach is to manually assign a primary lane to each champion and group them through that. For this, I used http://na.op.gg/champion/statistics to look at the different champions in each role.

## The Clusters

After going through the champion roster and assigning each champion a main role, I was able to use K-means clustering to assess the different types of playstyles within each role. This means that I was able to see the different types of champions being played in the top lane, jungle, mid lane, bottom lane, and support. 

In the section below, I will be providing the number of clusters (play-styles) in each lane, the types of champions in each cluster, and a high level assessment on the differentiating factor of the clusters.

### Top Lane

<p align="center">
  <u><b> Top Lane Number of Clusters </b></u>
</p> 
<p align="center">
  <img src="./Images/Top Lane Elbow.png" title="Elbow">
</p>

Based on the elbow plot above, it is fairly clear that the point of diminishing returns begins at k = 3 but really becomes evident at k = 4. For this reason, I have settled on having 4 clusters for the top lane.

<p align="center">
  <u><b> Top Lane Clusters </b></u>
</p> 
<p align="center">
  <img src="./Images/Top Lane Cluster.png" title="Cluster">
</p>

As you can see from the table above, the 4 clusters have pretty distinguishing features. 

From a high level perspective, cluster 1 contains champions that are generally tanky and have a form of cc. 

Cluster 2 contains champions that have a high amount of sustain in lane or an ability that instantly gives them more HP through transformations. 

Cluster 3 contains champions that either are lane bullies or late game scalers. The commonality between those 2 are that they are likely to get many kills in a game. This is why this cluster averages the most kills out of all the clusters. 

The last cluster is interesting as it contains just three champions. The commonality with these champions is that they run at you and are considered to be "feast or famine". These champions are one-dimensional and if they don't get ahead in the early game, they are incredibly easy to shut down, especially at the highest level of play.

### Jungle

<p align="center">
  <u><b> Jungle Number of Clusters </b></u>
</p> 
<p align="center">
  <img src="./Images/Jungle Cluster.png" title="Elbow">
</p>

Based on the elbow plot above, it is fairly clear that the point of diminishing returns begins at k = 3. For this reason, I have settled on having 3 clusters for the jungle.

<p align="center">
  <u><b> Jungle Clusters </b></u>
</p> 
<p align="center">
  <img src="./Images/Jungle.png" title="Jungle">
</p>

Based on the average values from each cluster it is evident that cluster 1 contains the high damage, carry-style junglers. 

On average, champions in cluster 1 get two more kills than the champions in the other clusters. They also average 3k more damage over the course of a game over other junglers as well. 

Champions in cluster 2 fit the mold of being more supportive and provide crowd-control capabilities to the team. As such, these champions average the lowest gold of the clusters as well as a crowd control of ~46 while the other 2 clusters average a crowd control score of ~13 and ~20. 

The last cluster contain the tanks. Some of these champions are not necessarily tanks by nature, but they are considered to be early game champions that fall off in the late game, such as Xin Zhao, Lee Sin, and Reksai. This is why these champions start to build tanky items to maintain value to their team. As such, the champions in this cluster average the lowest damage out of the clusters and the highest damage taken. This cluster averages over 20% more damage taken then the next highest cluster. 

### Middle

<p align="center">
  <u><b> Mid Lane Number of Clusters </b></u>
</p> 
<p align="center">
  <img src="./Images/Mid Lane Elbow.png" title="Elbow">
</p>

Based on the elbow plot above, it is fairly clear that the point of diminishing returns begins at k = 3. For this reason, I have settled on having 3 clusters for the mid lane.

<p align="center">
  <u><b> Mid Lane Clusters </b></u>
</p> 
<p align="center">
  <img src="./Images/Mid Lane.png" title="Mid Lane">
</p>

Based on the selection of champions in each cluster alone, it is fairly evident what the playstyles and distinguishing play-patterns are within each cluster. 

Looking at cluster 1, these champions are predominantly mages with long range and some form of crowd-control. As such, champions in this cluster average a crowd-control score of 23.14 while the other 2 clusters hover around 14. Champions in this cluster are also a main source of damage for any team composition, however, they don't need kills to be useful. As such, they average 1 whole kill less than the other main cluster, cluster 2, but they average a higher kill/death/assist ratio. 

Champions in cluster 2 are mostly assassins. This means they are high risk - high reward and often need to get up close and personal with the enemy in order to deal their damage. Since they expose them to the enemy team, they average the most damage taken out of the other 2 groups in the mid lane. They average over 23% more damage taken then the next closest cluster. As I mentioned before, this group averages the most kills out of the clusters, averaging upwards of 6 kills a game. This is due to the fact that earning kills nets you more gold and you need the gold to maintain your advantage state in the game. If an assassin falls behind in the game, their team is pretty much guaranteed to lose. Earning more kills nets you more gold individually which is why these assassin champions average a lot more. 

The last cluster is pretty interesting as it only contains one champion. This champion, Ziggs, has a few unique qualities in that he is long range and has abilities to take down enemy structures, like towers, very quickly. As such, Ziggs averages more than 6x the damage dealt to enemy structures per game than any other cluster in the mid lane. Because all of Ziggs abilities are area-of-effect, he also averages the most damage to objectives and as a whole out of the other clusters. This is why he is a cluster of 1 because Ziggs is essentially an all encompassing champion.

### Bot Lane

<p align="center">
  <u><b> Bot Lane Number of Clusters </b></u>
</p> 
<p align="center">
  <img src="./Images/Bot Lane Elbow.png" title="Elbow">
</p>

Based on the elbow plot above, it is fairly clear that the point of diminishing returns begins at k = 4. For this reason, I have settled on having 4 clusters for the mid lane.

<p align="center">
  <u><b> Bot Lane Clusters </b></u>
</p> 
<p align="center">
  <img src="./Images/Bot Lane.png" title="Bot Lane">
</p>

The bot lane is interesting as the playstyle of ADCs tend to be very similar. Their duty is to dish out as much damage while taking the least damage possible due to their squishy nature. However, looking at the clusters in the bot lane, it makes sense that the role is segmented this way. 

Looking at cluster 1, these champions tend to be more supportive in nature by providing crowd control capabilities to their team. The data corroborates this assessment as they average the highest assists out of the bot lane clusters, averaging nearly 10% more than then next closest cluster and they average the highest crowd controls scores as well, averaging nearly 41% more than the next closest cluster. 

Looking at cluster 2, the champions in this cluster all control the space around them, either with abilities that displace enemies or through traps that prevent enemies from approaching or the ability to move large distances themself to create more space between them and the enemy. The champions in this cluster are mostly lane-bullies with the ability to shove lane and oppress the other side. As such, they average the most damage out of all the clusters to objectives and turrets. This is because they are able to obtain lane priority. 

Looking at cluster 3, these champions have the shortest range and are auto attack reliant. Since they need to get up close to the enemy in order to deal damage, they average the highest damage taken out by 12% from the next closest cluster. On the other-side, they also average the most kills out of the other ADCs, but only by a marginal amount.

Looking at the final cluster, this cluster only contains one champion who has the most unique playstyle out of the marksmen. Ezreal is a poke based marksman who primarily relies on his spells to deal damage. As such, it makes sense that he averages the least amount of kills but the most damage dealt per game. 

### Support

<p align="center">
  <u><b> Support Number of Clusters </b></u>
</p> 
<p align="center">
  <img src="./Images/Support Elbow.png" title="Elbow">
</p>

Based on the elbow plot above, it is fairly clear that the point of diminishing returns begins at k = 6. For this reason, I have settled on having 6 clusters for support.

<p align="center">
  <u><b> Support Lane Clusters </b></u>
</p> 
<p align="center">
  <img src="./Images/Support.png" title="Support Lane">
</p>

Support has the highest number of clusters out of any other lane indicating the various types of play-styles this role can play. Based on the champions in the different clusters and my domain knowledge, being a bot lane main, I can say that each cluster is definitely distinctive in it's play-style. 

Looking at cluster 1, these champions primarily provide heals and sheilds to you and your team. Their main duty is to empower you to deal more damage. As such, they average the least kills and the highest vision score - which is their primary duty as a supportive style support.

Looking at cluster 2, these champions have a high amount of damage negating abilities. As such, the champions in this cluster average the highest amount of damage taken than any other cluster.

Looking at cluster 3, these long ranged high damage dealer mages purpose is to poke the enemy bot lane down so your bot laner can free farm. As such, these champions average the highest kills and damage dealt out of the other clusters for supports. This cluster averages just over 36% more damage dealt than the best cluster.

Looking at cluster 4, this cluster contains the hybrid champions of clusters 1 and 3. These champions help prevent their damage dealers from getting locked down in a fight through their supportive abilities as well as have high damaging skills to poke down their lane opponents. The data I have does not accurately represent the value these types of champions bring to a team and thus this cluster has stats that make this the average for supports.

Looking at cluster 5, these are the tanky high crowd control type of supports. As such, they average the second highest damage taken, second highest crowd control, as well as the least damage dealt.

Looking at the last cluster, these champions are the long range poke based champions which big shields/heals. As such, they average the second most damage done and the least damage taken out of all the clusters for support. They also average the highest crowd control score by the number of slows and roots in their kit. In my opinion, these 2 champions are definitely the most annoying to lane against as these champions, by nature, prevent the enemy from doing anything while dealing damage.

## Deep Dive on Qiyana and Cluster 2 of the Mid Lane

<p align="center">
  <u><b> Cluster 2 Stat Scores </b></u>
</p> 
<p align="center">
  <img src="./Images/Assassin Scores.png" title="Assassins">
</p>

<p align="center">
  <u><b> Distribution of Qiyana Scores </b></u>
</p> 
<p align="center">
  <img src="./Images/Qiyana Plot.png" title="Assassins">
</p>

<p align="center">
  <u><b> Distribution of Zed Scores </b></u>
</p> 
<p align="center">
  <img src="./Images/Zed Plot.png" title="Assassins">
</p>

<p align="center">
  <u><b> Distribution of Talon Scores </b></u>
</p> 
<p align="center">
  <img src="./Images/Talon Plot.png" title="Assassins">
</p>

<p align="center">
  <u><b> Distribution of Yasuo Scores </b></u>
</p> 
<p align="center">
  <img src="./Images/Yasuo Plot.png" title="Assassins">
</p>