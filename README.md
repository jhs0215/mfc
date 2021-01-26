# mfc
mfc study

 ## MFC 확장 DLL 만들기 

 - 전처리 정의에 따라서 컴파일 옵션이 달라진다.  
 _USRDLL, _AFXDLL, _AFXEXT 의 조합에 의해서 일반 DLL, 확장 DLL, MFC 확장 DLL 이 결정된다. MFC DLL 프로젝트를 만들면 (vs2017 이상일때) 기본적으로 _USRDLL 이 정의되어 있다. _USRDLL 이 입력되어 있으면 MFC 확장 DLL 을 만들면 Resource 공유를 할 수 없다. _USRDLL 을 지우고 _AFXEXT 를 추가해야 Resource 공유 MFC 확장 DLL 을 생성할 수 있다. 
 Resource 공유 MFC 확장 DLL 을 생성하려면 DLLMain 함수를 새로 만들어야 한다. DLL_PROCESS_ATTACH 일 때에 AfxInitExtensionModule 를 초기화 해야하고 DLL_PROCESS_DETACH 일 때는 AfxTermExtensionModule 를 이용하여 라이브러리를 종료 해야 한다. DLLMain 함수는 MFC 에서 기본적으로 가지고 있기 때문에 MFC 정적 라이브러리 함수를 사용하고 있거나 App 을 이용하고 있으면(Visual Studio 에서 기본으로 생성 후 사용되고 있다.) 오류가 발생한다. 
Resource 공유를 위해 대표적으로 이용하고 있는 MFC 정적 라이브러리 함수 - AFX_MANAGE_STATE(AfxGetStaticModuleState()) - 가 있다. 확장 MFC DLL 을 만들 때 자주 발생하는 오류 이기 때문에 주의가 필요하다.

 - 확장 DLL 에서 만든 Class 를 사용하는 방법  
 Class 를 Dll 외부에서 사용하기 위해서는 __declspec(dllexport)/__declspec(dllimport) 키워드를 이용해야 한다. MFC 에서는 AFX_EXT_CLASS 매크로를 지원해주고 있다. 이 매크로는 전처리 정의에 따라서 키워드 정의가 달라지는데 확장 DLL 에서는 _AFXEXT 를 정의하면 된다. 
