# Netlogo-project-Crusades-

## WHAT IS IT?

The Crusades were a series of religious wars initiated, supported, and sometimes directed by the Latin Church in the medieval period. The best known of these Crusades are those to the Holy Land in the period between 1095 and 1291 that were intended to recover Jerusalem and its surrounding area from Islamic rule.

In 1095, Pope Urban II proclaimed the First Crusade at the Council of Clermont. He encouraged military support for Byzantine emperor Alexios I against the Seljuk Turks and called for an armed pilgrimage to Jerusalem. Across all social strata in western Europe, there was an enthusiastic popular response. The first Crusaders had a variety of motivations, including religious salvation, satisfying feudal obligations, opportunities for renown, and economic or political advantage. Later crusades were generally conducted by more organized armies, sometimes led by a king. However, some bands of crusaders acted on their own and sought only fortune and wealth in the Holy Land; relying on captains of fortune and clashing among themselves

The aim of this model is to depict -in a simplified fashion- this historical and cultural event. The agents are militias of Seljuk Turks and Christian believers who will wage wars, build alliances and protect heir allies in an abstract representation of medieval Jerusalem

## HOW IT WORKS

Each time unit represents the time required to decide on, plan and organize a military raid. Agents might seek alliances with other agents and wage wars against their rivals. Allied agents will decide whether to support their peers in their military endeavors or abandon them to their fate.

<b>The likelihood of adopting a cooperative or hostile approach depends on two properties: bellicosity and power. The more beligerant a agent is, the more likely it will pursue conflict. The more powerful one becomes, the more it will dissuade potential attackers and invite potential allies.</b>

Bellicosity and power change depending on agents’ actions. Power is always lost on combat, as even a successful campaign incur in expenses and attrition. Losers, however, lose much more. Bellicosity will increase each time an agent becomes involved in a war, and decrease by being in an alliance.

If two agents nurture an alliance for long enough, their bellicosity will drop to 0, as they grow complacent with the political security. If an agents gets left out, it might lash out against the others, growing ever more beligerant without an alliance to balance the lure of warfare.

Christian crusaders have an additional property: stability (Holy-League Cooperation). It represents their fractious nature, and their lack of organization and leadership. Stability acts as a discount factor to power: the more it drops, the more an agent’s power pool shrinks. On the other hand, when it is set from the beginning it acts as an empowering factor of 1.5 with respect their power.

<b> Agent properties </b>

* power - military strength. Determines the prospect of victory when waging war, and the likelihood of obtaining allies

* ally-power - military strength of allied agents

* war-effort - sum of power + ally-powers

* bellicosity - propensity to declare war.

* allies? - number of allies

* stability (crusaders only) - internal stability of the militias. Acts as a discount factor to calculate power. Increases after military victories, decreases after defeats.

<b> Environment </b>

The environment is a simple Voronoi diagram representing the battleground of the Holy Land. Actions occur within network segments involving agents connected by alliances and warfare links. Alliances are depicted in white, active wars in black. There is a third class of links, alliance requests, that do not appear on the map.

<b> Setup </b>

The setup button randomly distributes agents within the model space. The stability of crusaders actors is always set at 1.0. The ally count is always set at 0. The value of other properties (number of agents, bellicosity, power) is assigned based on inputs in the interface tab.

<b> Time step </b>

Each time unit is divided into the following steps:

Wars are cleared

* replenish-power - power is replenished and recalculated (eg. Christians’ stability factor)

* offer-alliance - agents decide whether to send an alliance request

* accept-alliance - agents who received an alliance request decide whether to accept it.

If they, they become allies.

* declare-war - agents decide whether or not to declare war on other agents.

* opt-out - allies of warmongering agents decide whether to contribute to the war effort.

If they decline, the alliance is dissolved

* wage-war - Warring agents “roll” their war-effort score against one another. Warfare alwas take a toll on power, but losers lose much more.

* Perish - agents with 0 power or less die

* refresh-territory - patches refresh the Voronoi diagram

Alliance-requests are cleared

The model stops if all agents of a given breed perish. However, it may also come to a standstill if they reach a cooperative equilibrium (i.e. nobody attacks anybody). This gridlock can be broken by the modeler with the help of some tools in the interface. 

## HOW TO USE IT?

The interface allows the modeler to adjust the model’s critical parameters: number of agents, power and bellicosity of each breed.

The “Holy-league cooperation” switch activates or de-activates the stability variable for the Christian agents.

The “warfare” chooser determines whether agents are allowed to declare war on agents of the same breed.

There are provided buttons (e.g. "Send aid from ..." or "Increase the heatred towards...") that allow the modeler to interact with model troughout the simulation, increasing respectively, either agents' power and bellicosity or killing all alliances (e.g. "Clear alliances")

The output graphs plot the overall power of agents, the number of agents survived, total number of alliances and active wars. The latter one is useful to indicate if it is necessary to provoke agents. Agent distribution can vary in significant ways depending on the choice of parameters. One breed might dominate the entire map, or reach a cooperative equilibrium with the other. 

## THINGS TO TRY

It might be interesting to model the schism that happened between the Orthodox Church and the Roman Catholic Church and how it influenced progresses in the military campain. Also because at the end of the first Crusade, the distribution of land between Christians and Byzantines was not fair. 

It might be worth trying to add some other agents' attributes more political/consensus related.




## REFERENCES


Wilensky, U. (2006). NetLogo Voronoi model. [http://ccl.northwestern.edu/netlogo/models/Voronoi](http://ccl.northwestern.edu/netlogo/models/Voronoi). 
      
[Crusades - Britannica](https://www.britannica.com/event/Crusades)



# -------------

Manuel Scionti, University of Catania 2022, Data Science for Management 
