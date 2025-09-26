# 과제 6번(도전과제까지 진행)

## 1. Moving Actor (SetActorLocation)  
  ![MovingActor](https://github.com/user-attachments/assets/2a4f725d-12d6-4e44-9041-da9ce6bc23f5)
  ```c++
  FVector DeltaLocation = MovingDirection.GetSafeNormal() * MovingSpeed * DeltaTime * (IsMoveToTarget ? 1 : -1);
  CurMovingDist += MovingSpeed * DeltaTime;
  
  if (CurMovingDist >= MovingDistance) {
  	IsMoveToTarget = !IsMoveToTarget;
  	CurMovingDist = 0;
  }
  
  SetActorLocation(GetActorLocation() + DeltaLocation);
  ```  

## 2. Rotating Actor (AddActorLocalRotation)  
  ![RotatingActor2](https://github.com/user-attachments/assets/4f1a9fe1-5fd8-4b17-9132-4a6db9067203)
  ```c++
  FRotator DeltaRot = RotDirection * RotSpeed * DeltaTime * (IsPositiveSide ? -1 : 1);
  AddActorLocalRotation(DeltaRot);
  
  CurRotAmmount += RotSpeed * DeltaTime;
  if (CurRotAmmount >= MaxRotAmmount) {
  	// 회전중지
  	IsShouldRot = false;
  	CurRotAmmount = 0;
  
  	IsPositiveSide = !IsPositiveSide;
  }
  ```
## 3. Replaction System
<img width="326" height="235" alt="image" src="https://github.com/user-attachments/assets/12282bff-1d62-4aa3-bf75-99a334bdfe4f" />
<img width="347" height="151" alt="image" src="https://github.com/user-attachments/assets/23203b50-f4a8-48ca-91fa-19d55307afdb" />


## 4. Auto Return Rotating Actor (FTimerHandle)  
  ![RotatingActor](https://github.com/user-attachments/assets/40e8145c-af47-42c7-9042-37cacda540ac)
  ```c++
  if (IsAutoReturn && IsPositiveSide) {
  	GetWorld()->GetTimerManager().SetTimer(AutoReturnHandle, this, &ATrainingTarget::ExecRot, FMath::RandRange(1.f, 4.f), false);
  }
  ```

## 5. Random Location Spawn System
  ![GIF 2025-09-26 오후 1-54-35](https://github.com/user-attachments/assets/ba8633ea-1c9b-488b-8372-857a00947568)
  ```c++
  void AItemSpawner::SpawnItemAtRandomLocaion()
  {
  	UNavigationSystemV1* NavSystem = UNavigationSystemV1::GetNavigationSystem(GetWorld());
  
  	if (NavSystem == nullptr) return;
  
  	for (TSubclassOf<AActor> e : SpawnTargets) {
  		for (auto i = 0; i < 20; i++) {
  			FNavLocation LOC;
  			NavSystem->GetRandomPoint(LOC);
  
  			GetWorld()->SpawnActor<AActor>(e.Get(), LOC.Location, FRotator::ZeroRotator);
  		}
  	}
  }
  ```
