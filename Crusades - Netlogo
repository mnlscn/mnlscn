breed [ crusaders crusader ] ;the model is based on two breeds: muslims turks and christian crusaders
breed [ turks turk ]


undirected-link-breed [ alliances alliance ]
undirected-link-breed [ wars war ]                          ;links among agents
directed-link-breed [ alliance-requests alliance-request ]

globals [
power-turks ;; sum of the power of all turk agents
power-crusaders ;;   sum of the power of all christian agents
]

turtles-own [ ;all agents' properties
  power
  ally-power
  war-effort
  bellicosity
  allies?
]
crusaders-own [ stability ] ;christians only property



;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;; SETUP ;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;


to setup
  clear-all
  setup-agents
  ask turtles [
    set allies? count alliance-neighbors
  ]
  reset-ticks
end

to setup-agents
  set-default-shape crusaders "cross" ;setup crusader agents ("cross" custom shape)
  ask  n-of (num-crusaders) patches with [ count turtles-here = 0 ] [   ;; an alternative procedure to create turtles. Makes it easier to work with the Voronoi diagram
      sprout-crusaders 1[
        set color 45
        set stability 1.5
        set power crusaders-power * stability
        set size ( 1 + 2 * ( power / 100 ) ) ;; sizes matches the power meter. The stronger the agent, the bigger it is.
        set bellicosity crusaders-bellicosity
  ]
  ]
  set-default-shape turks "moon" ;setup turk agents ("muslim moon" custom shape)
  ask  n-of (num-turks) patches with [ count turtles-here = 0 ] [
      sprout-turks 1[
        set color 65
        set power turks-power
        set size ( 1 + 2 * ( power / 100 ) )
        set bellicosity turks-bellicosity
    ]
  ]
  ask patches [refresh-territory]
end


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;; GO ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;


to go
  ask wars [ die ]                                   ;; unlike alliance requests, wars are killed at the beginning of a tick, so we can see which ones have formed at each tick
  set power-turks  sum [ power ] of turks            ;; set up "power" as a global variable so it can be plotted
  set power-crusaders sum [ power ] of crusaders
  if not any? turks [user-message (word "Jerusalem has been freed! :)")]
  if not any? crusaders [user-message (word "Jerusalem has been captured! :(")]
  if not any? crusaders [ stop ]                     ;; if there are no agents of any one breed, stops the simulation and
                                                     ;; show a message about the outcome of the war
  if not any? turks [ stop ]
  ask turtles [
    set allies? count alliance-neighbors
    replenish-power
    offer-alliance                                  ;; setting several different procedures
    accept-alliance
    declare-war
    opt-out
    wage-war
    set size ( 1 + 2 * ( power / 100 ) )
    perish
  ]
  ask patches [
  refresh-territory
  ]
  ask alliance-requests [ die ]
  tick
end

;; Environment Procedures

to refresh-territory
  set pcolor [color - 3] of min-one-of turtles [distance myself]
end                                              ;; this is the code that generates the Voronoi diagram


;; Turtle Procedures

to replenish-power
  set power ( power + 10 )
  if power >= 150 [                              ;; power needs to be capped, otherwise it'd baloon indefinitely
    set power 150
  ]
  if breed = crusaders and Holy-league-cooperation? [ set power ( stability * power ) ]
end

to offer-alliance
    if allies? = 0 [
    if any? other turtles with [ allies? = 0 ] [
    ask one-of other turtles with [ allies? = 0] [
    create-alliance-request-from myself]
  ask alliance-requests [ set color yellow ]     ;; alliance-requests are colored yellow for debugging purposes only. Since they are killed the same time unit they are generated, we never get to see them
  ]
    ]
  if allies? > 0 [
    set bellicosity bellicosity - 0.1
    if bellicosity <= 0 [                        ;; bellicosity needs to be capped, otherwise it'd drop below 0.0
      set bellicosity 0
    ]
  ]
end

to accept-alliance
  if any? in-alliance-request-neighbors with [ power >= [power] of myself  ] [
    ask out-alliance-neighbors [ die ]           ;; agents suspend offers of alliance if they happen to receive a more favorable one.
                                                 ;;This is to ensure each agent gets only one ally.
    if any? alliance-request-neighbors [
    create-alliance-with one-of alliance-request-neighbors
    ]
  ask alliances [
    set color white
  ]
  ]
end


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;; MODALITIES OF WARFARE ;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;


to declare-war
  if warfare = "Every man for himself" [

    if any? other turtles with [ not alliance-neighbor? myself and power <= [ power ] of myself ] [              ;; agents won't wage wars with enemies more powerful than themselves
   if random-float 1.0 < bellicosity [                                                                           ;; the random factor that determines propensity to declare war
    ask one-of other turtles with [ not alliance-neighbor? myself and power <= [ power ] of myself ]
   [ create-war-with myself]
   ask wars [ set color black]
   ]
    ]
  ]
  if warfare = "Christians vs Muslims" [
    if any? other turtles with [ not alliance-neighbor? myself and power <= [ power ] of myself and breed !=[ breed ] of myself ] [
    if random-float 1.0 < bellicosity [
      ask one-of other turtles with [ not alliance-neighbor? myself and power <= [ power ] of myself and breed !=[ breed ] of myself ]
    [ create-war-with myself]
     ask wars [ set color black]
   ]
 ]
  ]
end


To opt-out
  if any? alliance-neighbors with [ any? war-neighbors ] [
    if any? alliance-neighbors with [ power < 0.5 * [power] of myself ] [      ;; if an agent gets too weak, its allies will abandon it.
    ask my-alliances [ die ]
  ]
  ]
end

to wage-war
  set ally-power sum [ power ] of alliance-neighbors
  set war-effort power + ally-power
  ask war-neighbors [
    ifelse ( [ war-effort ] of self ) * random-float 1.0 < ( [ war-effort ] of myself )     ;; computation that determines the outcome of wars
    [ set power ( power - random-float 30 )
      set bellicosity bellicosity + 0.1
      if bellicosity > 1 [ set bellicosity 1 ]
        if breed = crusaders and Holy-league-cooperation? [ set stability stability - 0.1 ] ;; Christian agents lose stability when defeated
    ]
    [ set power ( power - random-float 10 )                                                 ;; loss of power due to attrition and war expenses
      set bellicosity bellicosity + 0.1
      if bellicosity > 1 [ set bellicosity 1 ]
        if breed = crusaders and Holy-league-cooperation? [ set stability stability - 0.1 ]
    ]
  ]
end

to perish
    if power <= 0 [ die ]
end


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;; INTERFACE BUTTON PROCEDURES ;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;


to clear-alliances ;Kills all the alliances - useful to break a standstill
  ask alliances [ die ]
end

to provoke-turks  ;Increase hatred towards Christians
  ask turks [ set bellicosity bellicosity + 0.3 ]
end

to provoke-crusaders ;Increase hatred towards Muslims
  ask crusaders [ set bellicosity bellicosity + 0.3 ]
end

to crusaders-aid ;Send help from Venice and Genoa
  ask crusaders [ set power power + 10]
end

to turks-aid ;Send help from Balkans
  ask turks [ set power power + 10]
end
