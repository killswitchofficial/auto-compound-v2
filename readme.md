# PronteraReserve.sol
- ที่เก็บ ksw token ที่จะแจก โดยมีแค่ prontera, emperium ที่จะ withdraw ได้

# PronteraV2.sol
- Masterchef แจก เหรียญ ksw
- user สามารถ call deposit / depositToken / depositEther เพื่อฝากเข้าฟาร์มที่ต้องการ
- user สามารถ call withdraw / withdrawToken / withdrawEther / emergencyWithdraw เพื่อถอนออกจากฟาร์ม
- action ที่ต้องมีการ swap เหรียญ prontera จะส่งเหรียญเข้าไปที่ juno เพื่อ swap และ require ว่าต้องไม่เกิน deadline และ juno จะต้อง swap เงินให้ได้ไม่น้อยกว่า amountOutMin ที่ user expect
- juno เป็น proprietary ของ ksw ที่จะไม่เปิดเผย source code ไม่มีอะไรเกี่ยวกับ prontera ในเรื่องของ owner
- มี function storeAllowance, approveStore, increaseStoreAllowance, decreaseStoreAllowance, depositFor, storeKeepJellopy, storeReturnJellopy, storeWithdraw สำหรับให้ user approve เพื่อ stake share หรือมอบอำนาจให้ contract อื่น deposit และ withdraw share แทน
- owner = timelock 1 day
- juno guide = กระเป๋าธรรมดา

# Prontera approve flow
approveStore ทำงานเหมือน approve ของ ERC20 แต่จะทำให้ store ทำ action ได้ 2 อย่าง (ออกแบบมาเพื่อ staking + take profit / stop loss)
1. keep jellopy คือการเปลี่ยน user.jellopy เป็น user.storedJellopy (user จะถอนได้แค่ส่วน user.jellopy, user.storedJellopy จะถอนไม่ได้)
2. withdraw jellopy ได้เท่าที่จำนวนที่ user สั่งให้ store keep ส่วน pending ksw ตอนถอนจะส่งให้กับ user โดยอัตโนมัติ
store สามารถคืน jellopy ที่ keep ด้วยการเรียก storeReturnJellopy
- summary
user -> increaseStoreAllowance -> storeKeepJellopy -> storeWithdraw(optional)
```
ถ้า storedJellopy > 0 จะไม่สามารถ emergency withdraw ได้ ต้องให้ user เรียก storeReturnJellopy คืนกลับมาเป็น jellopy ให้หมดก่อน
```

# Emperium.sol
- Masterchef แจก เหรียญ ksw (สำหรับ stake ksw เพื่อให้ได้ ksw)
- ทำงานเหมือน masterchef ทั่วไปเพียงแค่เปลี่ยน logic per block เป็น per sec

# IzludeV2.sol
- ที่รับ token และคำนวน share ก่อนส่งต่อไป strategy farm (byalan)
- owner = timelock 1 day
- TVA = timelock 1 week (at least)

# PancakeByalanLP.sol
- strategy farm ของ Pancake lp
- hydra = กระเป๋าธรรมดา ใช้จัดการ pause, unpause, panic และ setGasPrice
- owner = timelock 1 day

# AllocKafra.sol
- ใช้สำหรับเช็คว่า user สามารถ deposit farm ปลายทางได้รึเปล่า โดยคิดจากจำนวน lp ที่ byalan ลงเทียกับขนาด total ของ farm ปลายทาง
- owner = timelock 1 day

# FeeKafra.sol
- ใช้สำหรับคิด fee ของ user เวลา withdraw
- owner = timelock 1 day

# GasPrice.sol
- ใช้สำหรับ limit gas price ของ tx เวลา call harvest
- owner = กระเป๋าธรรมดา

# Summary User flow
user -> deposit/withdraw -> pronteraV2