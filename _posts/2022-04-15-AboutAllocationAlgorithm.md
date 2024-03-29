---
layout: post
title: CSE Seat 자리 배정 알고리즘에 대해
---

현재 운영 중인 서비스는 완전 실시간 서비스로 운영 중이다. 하지만 처음 계획했을 당시 다음날 자리는 자리 신청 페이지를 통해서 신청하고, 저녁 10시에 모든 request에 대해 처리하기로 했다. 지금은 사용하진 않지만 완성된 자리 배정 알고리즘에 대해 이야기하겠다.<br/>
<br/>
자리 배정 알고리즘은 신청자들에 한해 적절히 자리를 배정한다. 알맞은 자리를 배정하기 위해 고려해야할 사항들이 있다.<br/>
1. 선착순
2. 1부, 2부
3. 원하는 자리
4. 원하는 호실
5. 친구 (본인 포함 최대 4명)

이렇게 총 다섯 종류가 있다.<br/>

여기서 제일 먼저 고려할 대상은 선착순이다. 제일 먼저 신청한 사람부터 원하는 이상적인 자리에 앉혀주는 게 맞다.<br/>
그다음 우선되어야할 것은 생각해봐야 할 주제가 꽤나 많았다. 따라서 고려했던 것들에 대해선 조금 뒤에 설명하기로 하고,
자리 선정은 점수제로 했다. 여러가지 고려사항에 대해 점수를 부여하고, 그 중 최대 점수인 자리로 선정한다.
자리를 선정하면 그 자리의 value는 -1이며, 앞으로의 자리 선택에서 배제한다.<br/>
<br/>
[자리 배정 알고리즘 전체 코드](https://raw.githubusercontent.com/CSE-seat/CSE-Seat/main/backend/src/AllocationAlgorithm.js)
<br/>
함수에 대한 설명<br/><br/>
Allocation<br/>                      각 request들에 대해 solveReq를 돌려서 나온 data를 통해 신청한 인원들을 자리를 배정해준다.<br/><br/>
solveReq<br/>                        request를 하나 받아서 자리 배정을 완료하고 배정된 자리에 대한 data를 반환한다.<br/><br/>
findNextUser<br/>                    solveReq 함수를 완료한 뒤 배정된 자리에 신청한 친구들을 채워넣는 과정에서 자리를 배정받을 친구를 고르는 함수<br/><br/>
selectSeats<br/>                     배정한 자리에 대해 더이상 고려하지 못하도록 자리를 배제하고, 자리 희망 인원, 빈 자리 개수에서 배정한만큼 빼는 함수.<br/><br/>
makePosition<br/>                    함수에 넣은 인원수만큼 방의 우선순위가 높거나 이상적인 자리를 내어준다.<br/><br/>
separateBacktracking<br/>            1부 또는 2부만 신청했고 makePosition 함수에서 이상적인 자리를 못 찾았을 때, 분리된 자리 중에서 이상적인 자리를 찾아주는 함수.<br/><br/>
separateExecptionBacktracking<br/>   1,2부 동시 신청했고 친구 신청했을 때, 1,2부 붙은 자리로만 줄 수 없어서 찢어진 자리로 줘야하는 상황에서 이상적인 자리를 찾아주는 함수.<br/><br/>
onlyException<br/>                   1,2부 동시 신청했고 혼자 신청했는 데, 붙은 자리가 없어서 1,2부 자리를 따로 배정해줘야할 때 이상적인 자리를 찾아주는 함수.<br/><br/>
settingHopeNumber<br/>               solveReq 함수를 하기 전 처리 단계. 모든 request에 대해 강의실을 원하는 인원을 산정한다.<br/><br/>
handleRoomApply<br/>                 방 번호로 주어지는 배열에 대해 각 index에 true or false로 넣어서 배열을 반환해주는 함수.<br/><br/>
handleEmptySeat<br/>                 1,2부, 각 강의실에 대해 빈 자리를 계산해서 배열에 넣어주는 함수.<br/><br/>
seatNumChanger<br/>                  자리 번호를 파라미터로 넣어주면 그 자리 번호에 대한 방 index, 2차원 자리 좌표를 배열에 넣어서 반환하는 함수.<br/><br/>
backToSeatNumber<br/>                방번호, 2차원 자리 좌표를 주면 자리 번호를 반환해주는 함수.<br/>
<br/><br/>
