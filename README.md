# ProjectileShooter

# Parent Classes
- Add a new C++ Class AActor type called Projectile
- Add a new C++ Class AActor type called Launcher

## Projectile Parent Class
- Header file
  - Declare a vector variable to be the impulse force for this projectile
  - Declare a setter to define the projectile impulse
  - Declare a Launch() function to launch the projectile with its impulse
```cpp
UCLASS()
class PROJECTILESHOOTER_API AProjectile : public AActor
{
	GENERATED_BODY()
	
public:	
	// Sets default values for this actor's properties
	AProjectile();

protected:
	// Called when the game starts or when spawned
	virtual void BeginPlay() override;

	void SetProjectileImpulse(FVector Impulse);

	void Launch(FVector ProjectileImpulse);

public:	
	// Called every frame
	virtual void Tick(float DeltaTime) override;

private:
	FVector ProjectileImpulse; 
};
```

- Implementation file
  -  Define projectile impulse setter 
  -  Define Launch()
    - Find the mesh in the projectile component and pass it to a pointer variable and add an impulse to it
  -  On BeginPlay() call Launch()

```cpp
AProjectile::AProjectile()
{
 	// Set this actor to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;
}

// Called when the game starts or when spawned
void AProjectile::BeginPlay()
{
	Super::BeginPlay();

	Launch(ProjectileImpulse);
}

// Called every frame
void AProjectile::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);
}

void AProjectile::SetProjectileImpulse(FVector Impulse)
{
	ProjectileImpulse = Impulse;
}

void AProjectile::Launch(FVector ProjectileImpulse)
{
	UMeshComponent* MyMesh = this->FindComponentByClass<UMeshComponent>();
	
	//If there is a mesh in the projectile component
	if (MyMesh != nullptr)
	{
		//Add impulse to this mesh 
			// bool = true so that mass has no influence on impulse
		MyMesh->AddImpulse(ProjectileImpulse, NAME_None, true);
	}
	else
	{
		UE_LOG(LogTemp, Warning, TEXT("Mesh not found"));
	}
}
```

## Launcher Parent Class
- Header file
  - Declare a projectile object and expose it to the blueprint
  - Declare a SpawnProjectile() function and expose it to the blueprint
  - #include "TimerManager.h"
  - Declare a StartTimer() function and pass in a float parameter to be the time range in between shots
  - Declare a FVector for the launcher location and a FRotator for the launcher rotation
  - Declare a timer handle variable

```cpp
UCLASS()
class PROJECTILESHOOTER_API ALauncher : public AActor
{
	GENERATED_BODY()
	
public:	
	// Sets default values for this actor's properties
	ALauncher();

protected:
	// Called when the game starts or when spawned
	virtual void BeginPlay() override;

	UFUNCTION() 
	void SpawnProjectile();

	void StartTimer(float ShootPause);

public:	
	// Called every frame
	virtual void Tick(float DeltaTime) override;

private:
	UPROPERTY(EditAnywhere, Category = "My Projectile")
	TSubclassOf<AActor> MyProjectile;

	FVector Location;
	FRotator Rotation;
	FTimerHandle TimerHandle; 

};
```

- Implementation file
  - Define SpawnProjectile() and make it spawn the projectile at the launcher location 
  - Define StartTimer() and pass SpawnProjectile() as its callback function
  - On BeginPlay() get launcher location and rotation
```cpp
ALauncher::ALauncher()
{
 	// Set this actor to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;
}

// Called when the game starts or when spawned
void ALauncher::BeginPlay()
{
	Super::BeginPlay();

	Location = GetActorLocation();
	Rotation = GetActorRotation();
}

// Called every frame
void ALauncher::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);
}

void ALauncher::StartTimer(float ShootPause)
{
	GetWorldTimerManager().SetTimer(TimerHandle, this, &ALauncher::SpawnProjectile, ShootPause, true);
}

void ALauncher::SpawnProjectile()
{
	GetWorld()->SpawnActor<AActor>(MyProjectile, Location, Rotation);
}
```

# Child Classes

## Projectile Child Class
- Create a C++ class derived from Launcher: "ProjectileChild_1"
- Create a Blueprint based on this class
  - Add a mesh to it
  - Check simulate physics

- Header File
  - Declare the constructor for this class
  - Declare a vector to be the impulse for this specific projectile and pass in its values in the X axis
```cpp
UCLASS()
class PROJECTILESHOOTER_API AProjectileChild_1 : public AProjectile
{
	GENERATED_BODY()
	
public:
	AProjectileChild_1();

private:
	FVector StrongImpulse = FVector(1000.0f, 0.0f, 0.0f); 
};
```

- Implementation file
  - Inside the constructor, call the setter for the projectile impulse inherited from the parent class passing in the impulse vector specific for this projectile
```cpp
AProjectileChild_1::AProjectileChild_1()
{
    SetProjectileImpulse(StrongImpulse);
}
```

## Launcher Child Class
- Create a C++ class derived from Launcher: "LauncherChild_1"
- Create a Blueprint based on this class
  - Add a mesh to it
  - Check simulate physics
  - Mark collision as "no collision"
  - in the projectile slot exposed in the C++ Launcher class, select the projectile blueprint that this specific class will launch.
- Add the launcher BP to the world

- Header File
  - Declare a float variable to be the specific launching speed for this child class
  - Declare BeginPlay() make it override the parent function
```cpp
  UCLASS()
class PROJECTILESHOOTER_API ALauncherChild_1 : public ALauncher
{
	GENERATED_BODY()

protected:
	void BeginPlay() override;

private:
	float ShootPauseFast = 0.3f;
};
```

- Implementation file
  - Define BeginPlay() extending from the parent class and inside it call StartTimer() inherited from the parent class passing the StartTime variable specific for this class 
```cpp
void ALauncherChild_1::BeginPlay()
{
    Super::BeginPlay();

    StartTimer(ShootPauseFast);
}
```

