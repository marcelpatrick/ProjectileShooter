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
