# Self-driving Trailer Agent travelling from Nairobi to Kisumu

## PEAS Framework Design
**Performance Measure** - Path Cost, Cargo & Trailer condition, Traffic rules obedience.
**Environment** - Kenya's Highway Network, Road traffic, GPS Information, Traffic rules, Weather Conditions.
**Actuators** - Steering wheel, Brakes, Accelerator, Indicators.
**Sensors** - Kenya's Road Map database, GPS, Cameras, Speed sensors, Fuel sensors, Collision detectors, Weather info.

## Agent Environment Definitions
1. **Partially Observable** - The agent can't perceive ahead of other traffic, nor through bad weather conditions and turns.
2. **Stochastic** - Road traffic and weather conditions are unpredictable.
3. **Sequential** - Current driving actions and decisions such as which routes are taken affect the  future states.
4. **Dynamic** - The environmental state such as weather and traffic changes as the agent is deciding and acting on it.
5. **Multi-agent** - The trailer agent interacts with other vehicles in its environment.

##### Nodes considered
1. Nairobi 
2. Maai Mahiu
3. Narok
4. Nakuru
5. Bomet
6. Eldama Ravine
7. Londiani
8. Kapsabet
9. Kericho
10. Muhoroni
11. Chavakali
12. Sondu
13. Kisii
14. Ahero
15. Kisumu


### Edges combined with Path Costs
| From          | To            | Distance (km) | Road(s)  |
| ------------- | ------------- | ------------- | -------- |
| Nairobi       | Maai Mahiu    | 53.8          | A8 & B3  |
| Maai Mahiu    | Narok         | 89.1          | B3       |
| Maai Mahiu    | Nakuru        | 106           | A8       |
| Narok         | Bomet         | 81            | B3       |
| Nakuru        | Eldama Ravine | 63.1          | B4 & C55 |
| Eldama Ravine | Kapsabet      | 107           | A8 & C36 |
| Nakuru        | Londiani      | 64.9          | A8       |
| Londiani      | Kapsabet      | 116           | A8 & C36 |
| Londiani      | Kericho       | 49            | B1       |
| Londiani      | Muhoroni      | 59.5          | C35      |
| Kapsabet      | Chavakali     | 48.3          | C39      |
| Chavakali     | Kisumu        | 31.9          | A1       |
| Bomet         | Sondu         | 78.6          | B3       |
| Bomet         | Kisii         | 82.4          | B3       |
| Bomet         | Kericho       | 76.1          | B3 & C23 |
| Sondu         | Kisumu        | 52.6          | A1 & B1  |
| Kisii         | Sondu         | 61.6          | A1       |
| Kericho       | Sondu         | 45            | C25      |
| Kericho       | Muhoroni      | 39.6          | B1       |
| Muhoroni      | Kisumu        | 60.7          | B1       |
| Muhoroni      | Ahero         | 37.7          | B1       |
| Ahero         | Kisumu        | 23.5          | A1       |


## Directed Graph
```mermaid
graph LR
    NBO[Nairobi]
    MMH[Maai Mahiu]
    NAR[Narok]
    NKU[Nakuru]
    BOM[Bomet]
    ELD[Eldama Ravine]
    KAP[Kapsabet]
    LON[Londiani]
    KER[Kericho]
    MUH[Muhoroni]
    CHA[Chavakali]
    KIS[Kisumu]
    SON[Sondu]
    KISII[Kisii]
    AHE[Ahero]

    NBO -->|"53.8 km A8 & B3"| MMH
    MMH -->|"89.1 km B3"| NAR
    MMH -->|"106 km A8"| NKU
    NAR -->|"81 km B3"| BOM
    NKU -->|"63.1 km B4 & C55"| ELD
    ELD -->|"107 km A8 & C36"| KAP
    NKU -->|"64.9 km A8"| LON
    LON -->|"116 km A8 & C36"| KAP
    LON -->|"49 km B1"| KER
    LON -->|"59.5 km C35"| MUH
    KAP -->|"48.3 km C39"| CHA
    CHA -->|"31.9 km A1"| KIS
    BOM -->|"78.6 km B3"| SON
    BOM -->|"82.4 km B3"| KISII
    BOM -->|"76.1 km B3 & C23"| KER
    SON -->|"52.6 km A1 & B1"| KIS
    KISII -->|"61.6 km A1"| SON
    KER -->|"45 km C25"| SON
    KER -->|"39.6 km B1"| MUH
    MUH -->|"60.7 km B1"| KIS
    MUH -->|"37.7 km B1"| AHE
    AHE -->|"23.5 km A1"| KIS
```

## Search Tree
```mermaid
graph TD
    NBO["Nairobi (0)"] -->|53.8| MMH["Maai Mahiu (53.8)"]
    MMH -->|89.1| NAR["Narok (142.9)"]
    MMH -->|106| NKU["Nakuru (159.8)"]
    
    NAR -->|81| BOM["Bomet (223.9)"]
    BOM -->|78.6| SON["Sondu (302.5)"]
    SON -->|52.6| KIS["Kisumu (355.1)"]
    BOM -->|82.4| KISII["Kisii (306.3)"]
    KISII -->|61.6| SON2["Sondu (367.9)"]
    SON2 -->|52.6| KIS2["Kisumu (420.5)"]
    BOM -->|76.1| KER["Kericho (300.0)"]
    KER -->|45| SON3["Sondu (345.0)"]
    SON3 -->|52.6| KIS3["Kisumu (397.6)"]
    KER -->|39.6| MUH["Muhoroni (339.6)"]
    MUH -->|60.7| KIS4["Kisumu (400.3)"]
    MUH -->|37.7| AHE1["Ahero (377.3)"]
    AHE1 -->|23.5| KIS1["Kisumu (400.8)"]
    
    NKU -->|63.1| ELD["Eldama Ravine (222.9)"]
    ELD -->|107| KAP["Kapsabet (329.9)"]
    KAP -->|48.3| CHA["Chavakali (378.2)"]
    CHA -->|31.9| KIS5["Kisumu (410.1)"]
    
    NKU -->|64.9| LON["Londiani (224.7)"]
    LON -->|49| KER2["Kericho (273.7)"]
    KER2 -->|45| SON4["Sondu (318.7)"]
    SON4 -->|52.6| KIS6["Kisumu (371.3)"]
    KER2 -->|39.6| MUH2["Muhoroni (313.3)"]
    MUH2 -->|60.7| KIS7["Kisumu (374.0)"]
    MUH2 -->|37.7| AHE2["Ahero (351.0)"]
    AHE2 -->|23.5| KIS11["Kisumu (374.5)"]
    LON -->|59.5| MUH3["Muhoroni (284.2)"]
    MUH3 -->|60.7| KIS8["Kisumu (344.9)"]
    LON -->|116| KAP2["Kapsabet (340.7)"]
    KAP2 -->|48.3| CHA2["Chavakali (389.0)"]
    CHA2 -->|31.9| KIS9["Kisumu (420.9)"]
    
    MUH3 -->|37.7| AHE["Ahero (321.9)"]
    AHE -->|23.5| KIS10["Kisumu (345.4)"]
```

**Note on Abstraction:** The nodes, and subsequently the paths selected, are the ones we found most suitable and logical for the journey. Any irrelevant nodes e.g. Eldoret, Oyugis which weren't chosen do not appear in the graph, tree nor path cost calculations.
## Optimal Path from Nairobi to Kisumu

**Path cost function:** Sum of distances between consecutive towns (in km).

The directed graph produces the following simple directed paths from Nairobi to Kisumu.

| #   | Path                                                                                  | Calculation (km)                             | Total (km)         |
| --- | ------------------------------------------------------------------------------------- | -------------------------------------------- | ------------------ |
| 1   | Nairobi -> Maai Mahiu -> Nakuru -> Londiani -> Muhoroni -> Kisumu                     | 53.8 + 106 + 64.9 + 59.5 + 60.7              | **344.9**(fastest) |
| 2   | Nairobi -> Maai Mahiu -> Nakuru -> Londiani -> Muhoroni -> Ahero -> Kisumu            | 53.8 + 106 + 64.9 + 59.5 + 37.7 + 23.5       | 345.4              |
| 3   | Nairobi -> Maai Mahiu -> Narok -> Bomet -> Sondu -> Kisumu                            | 53.8 + 89.1 + 81 + 78.6 + 52.6               | 355.1              |
| 4   | Nairobi -> Maai Mahiu -> Nakuru -> Londiani -> Kericho -> Sondu -> Kisumu             | 53.8 + 106 + 64.9 + 49 + 45 + 52.6           | 371.3              |
| 5   | Nairobi -> Maai Mahiu -> Nakuru -> Londiani -> Kericho -> Muhoroni -> Kisumu          | 53.8 + 106 + 64.9 + 49 + 39.6 + 60.7         | 374.0              |
| 6   | Nairobi -> Maai Mahiu -> Nakuru -> Londiani -> Kericho -> Muhoroni -> Ahero -> Kisumu | 53.8 + 106 + 64.9 + 49 + 39.6 + 37.7 + 23.5  | 374.5              |
| 7   | Nairobi -> Maai Mahiu -> Narok -> Bomet -> Kericho -> Sondu -> Kisumu                 | 53.8 + 89.1 + 81 + 76.1 + 45 + 52.6          | 397.6              |
| 8   | Nairobi -> Maai Mahiu -> Narok -> Bomet -> Kericho -> Muhoroni -> Kisumu              | 53.8 + 89.1 + 81 + 76.1 + 39.6 + 60.7        | 400.3              |
| 9   | Nairobi -> Maai Mahiu -> Narok -> Bomet -> Kericho -> Muhoroni -> Ahero -> Kisumu     | 53.8 + 89.1 + 81 + 76.1 + 39.6 + 37.7 + 23.5 | 400.8              |
| 10  | Nairobi -> Maai Mahiu -> Nakuru -> Eldama Ravine -> Kapsabet -> Chavakali -> Kisumu   | 53.8 + 106 + 63.1 + 107 + 48.3 + 31.9        | 410.1              |
| 11  | Nairobi -> Maai Mahiu -> Narok -> Bomet -> Kisii -> Sondu -> Kisumu                   | 53.8 + 89.1 + 81 + 82.4 + 61.6 + 52.6        | 420.5              |
| 12  | Nairobi -> Maai Mahiu -> Nakuru -> Londiani -> Kapsabet -> Chavakali -> Kisumu        | 53.8 + 106 + 64.9 + 116 + 48.3 + 31.9        | 420.9(slowest)     |


The least-cost path is:
**Nairobi -> Maai Mahiu -> Nakuru -> Londiani -> Muhoroni -> Kisumu**

**Total cost:**  
53.8 + 106 + 64.9 + 59.5 + 60.7 = **344.9 km**

This is the optimal route because it has the lowest cumulative path cost among all possible paths from Nairobi to Kisumu. 