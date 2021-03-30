# 1stJTWMercPoC
용병서버 파일기록 PoC

PoC니깐 머 쓰던 MOOSE 그냥 쓰고 
(CLIENT detect 하는 코드 그냥 복붙)

JSON 으로 기록할거고 (Lua Table -> JSON)
역시 귀찮으니깐 LFS OS IO 다 풀건데 원래 서버는 어케 하는건지? 데디서버는 이거 필요 없나?

하여간 귀찮아도 클라 이름으로 파일기록 하는게 좋겠다 (안그럼 괜히 메모리 먹으니깐..)


작동을 위해서는 DCS설치폴더/Scripts/MissionScripting.lua 에서
os, io, lfs 모듈 sanitize 를 주석처리해야한다.
아래처럼..

--Initialization script for the Mission lua Environment (SSE)

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
