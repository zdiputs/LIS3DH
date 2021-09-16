# LIS3DH
①室温对应adc值求出Rtx
Rtx = Temp_Amb_Resistance_calc(Vrx);

②室温查表
Tamb = (num-41) - (Rtx - Rtx_ref[num])/(Rtx_ref[num] - Rtx_ref[num+1]);

③室温Tamb代入25摄氏度室温下的物温表中找到Vtp_offs
Vtp_offs = Vtp_Thermopile_voltage_lookup(Tamb);
Vtp_offs = Vtp_r25[i-1]+(Vtp_r25[i]-Vtp_r25[i-1])*(Tamb-i+41); 

④梯度？ 室温敏感度因素 不知道什么含义
曲线的梯度（与热电堆敏感度成正比）随着环境温度的增加而减小。温度越高，曲线就越平坦。请注意，特征曲线形式不受这两个事实的影响。事实1只改变曲线；事实2根据环境温度压缩曲线的各个延伸。曲线特性仅取决于热电堆的过滤器特性。在大多数应用中，这种由于滤波器特性而引起的变化可以忽略不计。
TCsens:Temperature Coefficient of Thermopile Sensitivity
TC Factor which is TCF = 1 + Delta T × TCSENS
TCF = 1 + (Tamb - T25)*TCsens;

⑤修正过的热电堆电压
Vtp_corr = Vtp/TCF + Vtp_offs

⑥修正过的热电堆电压再代入25摄氏度室温下的物温表中找到Tobj
Tobj = Obj_Temperature_lookup(Vtp_corr);
Tobj = (i-41)+(Vtp_corr - Vtp_r25[i-1])/(Vtp_r25[i]-Vtp_r25[i-1]);
