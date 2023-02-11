# ProjectileShooter

# Parent Classes
- Add a new C++ Class AActor type called Projectile
- Add a new C++ Class AActor type called Launcher

## Projectile Parent Class
- Header file
  - Declare a vector variable to be the impulse force for this projectile
  - Declare a setter to define the projectile force

- Implementation file
  -  Define projectile impulse setter 
  -  On BeginPlay() get the mesh in this projectile component and add an impulse to it

## Launcher Parent Class
- Header file
  - Declare a projectile object and expose it to the blueprint
  - Declare a projectile spawner function and expose it to the blueprint
  - Declare a setter for the timer for the time range in between shots
  - Declare a float for the delta time between the shots 
  - Declare a FVector for the launcher location and a FRotator for the launcher rotation
  - Declare a timer handle variable

- Implementation file
  - Define SpawnProjectile() and make it spawn the projectile at the launcher location 
  - Define the timer setter and make it call SpawnProjectile()
  - On BeginPlay() get launcher location and rotation and start the timer

# Child Classes

## Launcher Child Class
- Create a C++ class derived from Launcher: "LauncherChild_1"
  - Declare a float variable to be the specific launching speed for this child class
  - Declare BeginPlay()
  - Define BeginPlay() extending from the parent class and inside it call StartTimer() inherited from the parent class passing the StartTime variable specific for this class 
- Create a Blueprint based on this class
  - Add a mesh to it
  - Check simulate physics
  - Mark collision as "no collision"
  - in the projectile slot exposed in the C++ Launcher class, select the projectile blueprint that this specific class will launch.
- Add the launcher BP to the world

## Projectile Child Class
- Create a C++ class derived from Launcher: "ProjectileChild_1"
- Create a Blueprint based on this class
  - Add a mesh to it
  - Check simulate physics



