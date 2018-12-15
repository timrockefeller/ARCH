title: Verilog 流水灯暂存
author: Kitekii
tags: []
categories: []
date: 2018-11-19 16:23:00
---
为了做这个奇妙的HDL实验而写的。
<!--more-->
flash_led_ctl.v
```verilog
module flash_led_ctl(
 	 	input clk,
 	 	input rst,
 	 	input dir,
 	 	input clk_bps,
 	 	output reg[15:0]led
 	 	);
 	 	always @( posedge clk or posedge rst )
 	 		if( rst )
 	 			led <= 16'h8000;
 	 		else
 	 			case( dir )
 	 				1'b0:  			 //从左向右
 	 					if( clk_bps )
 	 				 		if( led != 16'd1 )
 	 							led <= led >> 1'b1;
 	 						else
 	 							led <= 16'h8000;
 	 				1'b1:  			 //从右向左
 	 			 		if( clk_bps )
 	 						if( led != 16'h8000 )
 	 							led <= led << 1'b1;
 	 						else
 	 							led <= 16'd1;
 	 			endcase
endmodule
```
flash_led_top.v
```verilog
module flash_led_top(
        input clk,
 	 	input rst_n,
 	 	input sw0,
 	 	output [15:0]led
 	 	);
 	 	wire clk_bps;
 	 	wire rst;
 	 	assign rst = ~rst_n;
 	 	
 	 	counter counter(
 	 		.clk( clk ),
 	 		.rst( rst ),
 	 		.clk_bps( clk_bps )
 	 	);
 	 	flash_led_ctl flash_led_ctl(
 	 		.clk( clk ),
 	 		.rst( rst ),
 	 		.dir( sw0 ),
 	 		.clk_bps( clk_bps ),
 	 		.led( led )
 	 	);
endmodule
```