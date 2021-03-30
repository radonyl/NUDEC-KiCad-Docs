# 电赛KiCad教程

#### 介绍

KiCad是一款开源免费的PCB设计工具，得益于开源的优势，KiCad内置了由社区维护的丰富元器件库，从而免去反复画封装的痛苦。同时KiCad也是开放的，其文件基于文本格式储存，可以很方便的导入导出。在原理图绘制的部分，KiCad与各大EDA软件大致相同。在PCB绘制的部分工具和规则相比专业级工具要简单很多，这恰好避免初学者陷入复杂规则的“蜘蛛网”中。同时，KiCad也具有很高的社区热度。总的来说，KiCad是业余爱好者快速硬件设计的不二之选。

#### 文档结构

1.  软件操作注意事项
2.  快速绘制符号、封装
3.  PCB的阻抗控制
4.  PCB的规则检查
5.  导出用于制造的Gerber文件
6.  PCB送厂注意事项

#### 软件操作注意事项

1.  路径中只包括英文字母和下划线
2.  按住Ctrl(Windows) Cmd(OS X)可以把线走直
3.  PCB选中元器件一般要选择“封装”而不是焊盘，按F将元器件换层
4.  按ESC退出当前状态

#### 快速绘制符号、封装

1.  [原理图符号生成器](http://kicad.rohrbacher.net/quicklib.php)
2.  两种符号的绘制方式
    ![IMG](./images/readme/two_symbol_styles.png)
3.  使用阵列工具快速生成封装（选择焊盘后右键-创建阵列）
    ![IMG](./images/readme/use_array_to_create_footprint.png)

#### PCB的阻抗控制

1.  使用自带的“PCB计算器”
    ![IMG](./images/readme/kicad_pcb_calculator_impedance.png)
2.  在嘉立创做板，可以用[阻抗神器](https://www.sz-jlc.com/weixin/calculation/impedance.html)
3.  阻抗控制的走线下地平面必须完整
4.  使用共面波导+地平面的结构，需要沿着传输线打过孔
    ![IMG](./images/readme/coplanar_waveguide_via_stitching.gif)
5.  走线尽量短，转弯使用45度角


#### PCB的规则检查

1.  **投产前使用DRC检查是否存在违例**
2.  可依据制造商的工艺参数设置最小间距等
    ![IMG](./images/readme/kicad_pcb_drc.png)

#### 导出用于制造的Gerber文件

1.  详见图片
    ![IMG](./images/readme/kicad_pcb_plot_gerber.png)

    ![IMG](./images/readme/kicad_pcb_gen_drill_file.png)
    
2.  完成后，将工程目录的gerber文件夹（之前设置的输出文件夹）打包为.zip后，将zip文件送厂

#### PCB送厂注意事项

1.  再次确认板厚，是否与设计阻抗时的一致；如果是四层板，选择正确的层叠结构
2.  设计参数应高于极限参数
3.  对于非常规设计，选择“需要生产确认稿”
4.  对于贴片元件多的板子可以使用SMT，或开钢网

#### 常见PCB绘制问题

1.  使用偏脚的SMA接头没有放在板子的边缘，到时候就装不上去了！
    ![IMG](./images/readme/layout_sma_edge.png)

2.  GND铺铜的问题，对于二层板建议两面都铺上完整的地。对于四层板，在内层有完整铺铜的情况下，外层无需铺铜。这样的话板子上的地线就不需要单独走线了，只需要在接地的引脚附近打一个GND过孔（不要打在引脚焊盘上！），在画完之后检查所有的GND的连通性，可以通过添加过孔的方式解决连通性问题。
    ![IMG](./images/readme/kicad_pcb_gnd_pour_1.png)

    这也启示我们，如果是两层板，尽量不要在背面走长线，以免破坏地平面完整性。

    第二，我们一定要尽量保证所有芯片到地的阻抗最小，因此最好每个引脚都有GND过孔，当然靠得比较近的可以共用。

    ![IMG](./images/readme/kicad_pcb_gnd_pour_2.png)

3.  铺铜的设置
    ![IMG](./images/readme/kicad_pcb_pour_config.png)
    
4.  板上电源输入的问题，一般情况下通过间距为2.54mm的插针引入电源，电源接入后经过滤波给板子提供电源。可选电源指示灯、接反保护等。
    ![IMG](./images/readme/power_input_section.png)

    请注意，输入线路虽然名字不叫+5V，但也是电源线的一部分，走线宽度应与+5V保持一致。

5.  电源去耦，用于防止噪声通过芯片电源线进出，应该紧靠芯片放置

    ![IMG](./images/readme/kicad_sch_decouple_cap.png)

    ![IMG](./images/readme/kicad_pcb_decouple_cap.png)

6.  走线粗细的选择，普通信号线可选择0.2/0.25mm，电源线根据电流大小，一般在0.5mm~1mm。射频信号线，根据设计的阻抗走线，同时遇到线宽变化时，应尽量平滑过渡。

    ![IMG](./images/readme/kicad_pcb_powerline_example.png)

7.  走线的基本规则:不走直角，避免锐角；不走断头线，非必要不绕路。

    ![IMG](./images/readme/pcb_routing_rule_1.png)

8.  信号回流路径的考虑
    
    下图中底层绿线割裂了底层地平面，导致信号回流路径受阻。如果是四层板，也要同样注意回流路径，避免空间上不重叠（黄线是理想的回流路径，被割裂后被迫走蓝线）
    
    ![IMG](./images/readme/kicad_pcb_return_path.png)

9.  选择合适的网格

    ![IMG](./images/readme/kicad_pcb_grid.png)

10. 共面波导的Via Stitching

    ![IMG](./images/readme/kicad_pcb_via_stitching.png)