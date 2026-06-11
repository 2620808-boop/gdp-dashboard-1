import time
import random

angle_history = []
kick_standards = {
    "앞차기": [150, 180],
    "돌려차기": [155, 180],
    "옆차기": [160, 180]
}

print("=== 태권도 발차기 각도 분석 AI 시스템 ===")
print("\n[발차기 메뉴] 앞차기 / 돌려차기 / 옆차기")
selected_kick = input("연습할 발차기 종류를 입력하세요: ").strip()

if selected_kick not in kick_standards:
    print(f"⚠️ '{selected_kick}'은(는) 등록되지 않은 발차기입니다. 프로그램을 종료합니다.")
    exit()

min_target_angle = kick_standards[selected_kick][0]

print(f"\n📢 {selected_kick} 분석을 시작합니다. 카메라 앞에서 발차기를 해주세요!")
print("대기 중... (3초 후 시작)")
time.sleep(1)

frame = 1
is_camera_active = True

while is_camera_active:
    print(f"\n[프레임 {frame}/10] 실시간 분석 중...")
    
    is_person_detected = random.choice([True, True, True, True, True, True, True, True, True, False])
    
    if not is_person_detected:
        print("⚠️ 경고: 사용자가 화면에서 벗어났거나 인식이 끊겼습니다. 건너뜁니다.")
        frame += 1
        if frame > 10:
            is_camera_active = False
        time.sleep(0.5)
        continue
        
    current_angle = random.randint(90, 180)
    print(f"-> 현재 측정된 각도: {current_angle}°")
    
    angle_history.append(current_angle)
    
    frame += 1
    if frame > 10:
        is_camera_active = False
    time.sleep(0.3)

print("\n--- 🎬 발차기 분석 완료! 결과 정산 중 ---")
time.sleep(1)

if len(angle_history) == 0:
    print("❌ 에러: 유효한 발차기 데이터가 수집되지 않았습니다. 다시 시도해 주세요.")
else:
    max_angle = max(angle_history)
    print(f"\n[최종 리스트 분석 결과]: {angle_history}")
    print(f"🎯 당신의 이번 {selected_kick} 최고 각도는 [{max_angle}°] 입니다. (목표 기준: {min_target_angle}° 이상)")
    
    if max_angle >= min_target_angle:
        print("🥇 결과: [성공 (Great)]")
        print("-> 사범님 의견: 아주 훌륭한 높이입니다! 발을 뻗었을 때 무릎 펴짐과 중심 유지가 완벽합니다.")
    elif max_angle >= (min_target_angle - 15):
        print("🥈 결과: [노력 요함 (Good)]")
        print(f"-> 사범님 의견: 조금만 더 유연성을 기르면 완벽합니다! 약 {min_target_angle - max_angle}° 정도 더 높여보세요.")
    else:
        print("🥉 결과: [자세 교정 필요 (Keep Trying)]")
        print("-> 사범님 의견: 발차기 높이가 너무 낮습니다. 지지하는 디딤발의 축을 더 틀고 골반을 열어 차보세요.")

print("\n=======================================")
print("태권도 AI 시스템이 정상적으로 종료되었습니다.")