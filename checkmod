#!/bin/bash

while true; do
    # 모드 업데이트 확인
    screen -S pzserver -X stuff "checkModsNeedUpdate\n"

    # 로그 파일 모니터링
    tail -f ~/Zomboid/Logs/*DebugLog-server.txt -n 0 | while read line; do
        if echo $line | grep -q "updated"
        then
            echo "Zomboid mods are up to date"
            break
        fi

        if echo $line | grep -q "need update"
        then
            echo "Updating Zomboid mods"

            # 서버 업데이트 알림
            echo "Telling the server it's updating"
            screen -S pzserver -X stuff "servermsg "모드_업데이트가_감지되었습니다."\n"
            sleep 5    # 5초 대기 (5초)
            screen -S pzserver -X stuff "servermsg "5분_뒤_서버가_종료됩니다."\n"
            sleep 240  # 4분 대기 (240초)
            screen -S pzserver -X stuff "servermsg "1분_뒤_서버가_종료됩니다."\n"
            sleep 45   # 45초 대기
            screen -S pzserver -X stuff "servermsg "카운트_다운이_시작됩니다."\n"
            sleep 5   # 5초 대기
            # 10초부터 0초까지 카운트다운
            for i in {10..1}
            do
               screen -S pzserver -X stuff "servermsg "$i"\n"
               sleep 1
            done
            screen -S pzserver -X stuff "servermsg "재부팅을_시작합니다."\n"
            sleep 3

            # 서버 중지
            echo "Stopping Zomboid server"
            screen -S pzserver -X stuff "save\n"
            sleep 3
            screen -S pzserver -X stuff "quit\n"
            while pgrep ProjectZomboid > /dev/null; do
                sleep 1
            done

#            # 모드 및 게임 업데이트  (일단 보류사항)
#            echo "Updating Project Zomboid"
#            ~/Documents/Updates/PZupdate.sh

            # 서버 재시작
sleep 10
            echo "Starting Zomboid server"
            screen -S pzserver -X stuff "./start_server.sh\n"

            break
        fi
    done

    # 1분 대기 후 반복
    sleep 60
done
