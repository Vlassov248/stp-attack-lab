# stp-attack-lab
STP Attack Lab (Yersinia + Wireshark)

Учебный проект по информационной безопасности — исследование Layer 2 атак на протокол Spanning Tree Protocol (STP) в изолированной среде.  
Работа выполнена в Kali Linux с использованием Yersinia и Wireshark.

 Цели проекта

- Изучить механизмы работы STP и его уязвимости
- Провести реальные атаки (этично, в тестовой среде)
- Проанализировать поведение сети и выводы
- Зафиксировать результат в отчете

Используемые технологии

- Kali Linux + Yersinia
- Ubuntu + Wireshark
- VirtualBox (изолированная сеть)
- Промежуточный режим интерфейса

 Проведенные атаки

| Атака | Тип | Результат |
|------|-----|-----------|
| 0 | BPDU Flood | Успешно, Root ID заменён |
| 1 | TCN BPDU Injection | Успешно, отправка подтверждена |
| 2 | Fake Root Flood | Успешно, множество BPDU |
| 3 | TCN Flood | Успешно, реакция сети возможна |
| 4 | Root Role Spoof | Неуспешно, пакеты не прошли |
| 5 | Bridge Role Spoof | Неуспешно, без реакции |
| 6 | MITM Root Claim | Неуспешно, защита Ubuntu |

📄 **The additional PDF report** contains:
- Wireshark screenshots of each attack
- A more extended description of the processes
- Conclusions, protection analysis, recommendations
[Download PDF](./STP_Full_Report.pdf)
