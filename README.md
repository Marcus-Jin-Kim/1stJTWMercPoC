# 1stJTWMercPoC
용병서버 파일기록 PoC

그냥 Lua Table 을 serialize 해서 덤프한담에 do_file 로 읽어온다.
역시 귀찮으니깐 LFS OS IO 다 풀건데 원래 서버는 어케 하는건지? 데디서버는 이거 필요 없나?

하여간 귀찮아도 클라 이름으로 파일기록 하는게 좋겠다 (안그럼 괜히 메모리 먹으니깐..)


작동을 위해서는 DCS설치폴더/Scripts/MissionScripting.lua 에서
os, io, lfs 모듈 sanitize 를 주석처리해야한다.
아래처럼..

```--Initialization script for the Mission lua Environment (SSE)

dofile('Scripts/ScriptingSystem.lua')

--Sanitize Mission Scripting environment
--This makes unavailable some unsecure functions. 
--Mission downloaded from server to client may contain potentialy harmful lua code that may use these functions.
--You can remove the code below and make availble these functions at your own risk.

local function sanitizeModule(name)
	_G[name] = nil
	package.loaded[name] = nil
end

do
	--sanitizeModule('os')
	--sanitizeModule('io')
	--sanitizeModule('lfs')
	require = nil
	loadlib = nil
end
```

작동방식

1. 미션시작
2. 플레이어가 비행기에 탑승하면 이벤트 작동 playerName 감지
3. Balance[playerName] = 0으로 초기화
3. C:\Users\Username\Saved Games\ 폴더를 검색
4. playerName.lua 파일이 있는지 확인
5. 있으면 로드해서 Balance[playerName] 변수를 덮어쓴다
6. 따라서 이전에 파일에 기록된 값이 메모리에 올라옴
7. 출력
8. 보너스로 1원을 더해준다
9. 해당 파일에 저장

다음번에 로그인하면 이벤트가 작동하여, 이전에 기록된 Balance 를 꺼내오고
그에 이어서 .. 할수 있음

