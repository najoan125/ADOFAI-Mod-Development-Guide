## 목차
 - [Visual Studio 설치](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/dev0.md)
 - **프로젝트 기본 설정**
 - [메소드 패치](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/dev2.md)
 - [GUI 띄우기](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/dev3.md)
 - [모드 설정창](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/dev4.md)
 - [프로젝트 빌드](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/dev5.md)
 - [얼불춤 코드 보기](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/dev6.md)

## 1. 시작 전 준비물
 - [Visual Studio 2022](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/dev0.md)
 - 얼불춤 ( UMM이 설치된 상태 )
 - [.NET Framework 4.8](https://go.microsoft.com/fwlink/?linkid=2088517)
 - [dnspy](https://github.com/dnSpy/dnSpy/releases/download/v6.1.8/dnSpy-net-win64.zip)

## 2. 프로젝트 생성
![프로젝트생성](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/img/make.png?raw=true)
새 프로젝트 만들기 클릭     
    <br>
![선택](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/img/select2.png?raw=true)
클래스 라이브러리 (.NET Framework) 선택 후 다음 클릭     
프레임워크는 4.8을 추천합니다     
만약 클래스 라이브러리가 없다면 Visual Studio Installer에서 `.NET 데스크톱 개발`을 설치해 주세요    

## 3. 레퍼런스 참조
![참조](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/img/add.png?raw=true)      
맨 오른쪽에 있는 탭들중 `참조` 우클릭후 `참조 추가` 클릭    
    
`찾아보기`를 누른 후 아래에 있는 항목들을 모두 참조해주세요.
 - <얼불춤경로>/A Dance of Fire and Ice_Data/Managed/UnityModManager/0Harmony.dll
 - <얼불춤경로>/A Dance of Fire and Ice_Data/Managed/UnityModManager/UnityModManager.dll
 - <얼불춤경로>/A Dance of Fire and Ice_Data/Managed/Assembly-CSharp.dll


## 4. 셋업 만들기
![탭들](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/img/tabs.png?raw=true)     
프로젝트 생성될때 같이 생긴 Class1.cs을 우클릭 후 이름을 바꿔서 Main.cs라고 지정해줍니다. ( 꼭 Main일 필요 없음 )
```cs
public static class Main
{
}
```
그리고 Setup, OnToggle이라는 메소드도 같이 만들어주세요    
Setup은 UMM이 이 모드를 실행할때 처음 시작하는 메소드입니다

```cs
public static class Main
{
  public static UnityModManager.ModEntry.ModLogger Logger;
  public static Harmony harmony;
  public static bool IsEnabled = false;
  public static bool isplaying = false; //OnUpdate에 사용되기 위한 예시
  
  public static void Setup(UnityModManager.ModEntry modEntry)
  {
    Logger = modEntry.Logger;
    modEntry.OnToggle = OnToggle;
    modEntry.OnUpdate = OnUpdate; //선택
  }
  
  //선택
  private static void OnUpdate(UnityModManager.ModEntry modentry, float deltaTime)
  {
   //반복적으로 작동할 구문
    //예시
    if (!scrController.instance || !scrConductor.instance) //RDTools.dll, UnityEngine.dll, UnityEngine.CoreModule.dll 참조 필요
    {
	return; //모드가 실행될 때 로그에 NullPointerException이 뜨지 않도록 해줌
    }
    isplaying = !scrController.instance.paused && scrConductor.instance.isGameWorld; 
    //레벨을 플레이 중이고 일시정지 상태가 아니면 true, 레벨을 플레이 하고 있지 않거나 일시정지 상태면 false
  }
  
  private static bool OnToggle(UnityModManager.ModEntry modEntry, bool value)
  {
    IsEnabled = value;
    
    if (value)
    {
      //켜질때
      harmony = new Harmony(modEntry.Info.Id);
      harmony.PatchAll(Assembly.GetExecutingAssembly());
    }
    else
    {
      //꺼질때
      harmony.UnpatchAll(modEntry.Info.Id);
    }
    return true;
  }
}
```

[위 코드 복사하기](https://pastebin.com/hgDbmrjE)

### 짤막 팁
![빨강](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/img/redline.png?raw=true)     
위와 같이 빨간줄이 뜬다면 빨간줄이 뜬 텍스트에 마우스를 갖다대고 `Alt` + `Enter`를 눌러보세요    
![팁](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/img/altenter.png?raw=true)     

[[⬅]](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/dev0.md) [[➡]](https://github.com/najoan125/ADOFAI-Mod-Development-Guide/blob/main/ModdingGuide/dev2.md) (1/6)
