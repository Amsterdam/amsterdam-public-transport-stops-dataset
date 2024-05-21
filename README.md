
# Amsterdam Public Transport (OV) Elevator data

This repository is developed by the [Concept Team](https://rnd.amsterdam.nl) of the Innovation Department of Gemeente Amsterdam (Municipality of Amsterdam).

In this dataset, we have manually collected data from every Public Transport (OV) elevator that are within Amsterdam City borders, operated by the Municipality, GVB or NS. This includes transport connections from Tram, Metro & Train.

The dataset is created to determine the connections between elevators/exits/platforms at a station (nodes). The reason for doing this is to determine whether a user journey is possible when 1 of the elevators is out of service. In this case, the user is dependant of the use of elevators to achieve the goal of using Public Transport.

The data is collected for accessibility reasons and the researh is part of the project [Amsterdam4All](https://amsterdamintelligence.com/projects/amsterdam-for-all).
## Dataset

### JSON Files

The Dataset consists out of JSON files for every Public Transport (OV) stop that has an elevator at the station. The files are made to describe the node connections between elevators/exits/platforms. Here we will discuss what the JSON file consists out of.

- `id`: Custom ID.
- `name`: Name of the station.
- `city`: City where the elevator is located in.
- `stationLevels`: Used when a station consists out of multiple layers (floors or main halls).
    - `S1`: Custom name for station layer. 'S' indicating 'Station layer'.
    - `S_`: Next station layer (no particular order).
- `elevators`: List of elevators on every station.
    - `E1`: Indicates an elevator by ID. This ID corresponds to the ID that is within the elevator itself. 'E' indicating 'Elevator'.
    - `E_`: Next elevator (in order of ID).
- `platforms`: List of platforms.
    - `P1`: Indicates a platform. A platform can exist out of multiple types of OV vehicles. The next layer in the object Indicates this type.
        - `trainPlatforms`: Array indicating a set of platform sides where trains arrive. Example: `[ ["7", "West"] , ["8", "East"] ]` indicates 2 sides on the platform, platform number 7 where trains depart towards the West, and platform 8 where trains depart to the East. Directions can only be North, East, South or West.
        - `lines`: Array indicating a set of metros (subways) that arrive at this platform. Example: `[ ["50", ["Gein", "Isolatorweg"]] , ["51", ["Centraal Station", "Isolatorweg"]] ]` indicates 2 metros will arrive at this platform. Line 50 goes to either 'Gein' or 'Isolatorweg', while line 51 goes to either 'Centraal Station' or 'Isolatorweg'.
        - `busplatforms`: Array indicating a set of platform sides where busses arrive. Example: `[ ["15", "22"] ]` indicates this platform side is connected to bus line 15 and 22.
        - `tramPlatforms`: Array indicating a set of platform sides where trams arrive. Example: `[ ["19", "Diemen Sniep"] ]` indicates this platform side is connected to line 19 with end destination 'Diemen Sniep'.
    - `P_`: Next platform (no particular order).
- `exits`: List of exits at the station.
    - `EX1`: Indicates an exit by exit name.
    - `EX_`: Next exit (no particular order).
- `edges`: Indicates the [**Node**](#node_connections) connections. For every node ('station layer' `S_`, 'exit' `E_`, 'platform' `P_` or 'exit' `EX_`) there is an array that lists the other nodes that are accessible without the use of stairs, elevators or escalators. Example: `{ ..., "P7": ["E6", "E7", "EX2", "EX4"], ... }` indicates the node 'P7' has a direct connection with _Elevator 6_ ('E6'), _Elevator 7_ ('E7'), _Exit 2_ ('EX2') and _Exit 4_ ('EX4').

### Node connections

The `edges` key in the JSON file is the most important, since it tells us all the possible connections between nodes. For accessibility purposes, we can determine if a broken elevator between an exit and a platform is causing an accessibility problem. Some stations have platforms that are accessible using ramps or a second elevator. See the [example image](./nodes-example.png) for a clearer view.