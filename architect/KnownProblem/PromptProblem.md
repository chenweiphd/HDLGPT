# 问题汇总

说明：问题记录请参照模板，方便模型开发者了解问题的具体情况，以进行微调

//以下为记录模板

## 重复定义信号

### 问题描述

容易重复定义信号，例如模块定义中已经出现input XX后,又重复定义了一个reg XX。

### Prompt示例

[输入]

[输出]

[输入]

[输出]

### 正确输出应为

[输出]


### 问题分析

缺少xxx，通过xxx方法纠正后，仍出现xxxx

//以上为记录模板


## 重复定义信号

### 问题描述

容易重复定义信号，例如模块定义中已经出现input XX后,又重复定义了一个reg XX。

### Prompt示例

[输入]  
```verilog
I am trying to create a Verilog module called shift register.The definition of the module is shown below:    
"module shift_reg  
(  
input clk,  
input rst_n,  
input 	[7:0] datain_a,  
input 	enable,  
output  reg [7:0] data_out  
);"  
rst_n is the low effective reset signal. When the enable signal is set to 1, the datain_a are shifted one bit to the right and assigned to data_out.Write and explain verilog code that is consistent with the provided information.Be sure to use only synthesizable Verilog-2001 syntax.  
```
[输出]    
```verilog
module shift_reg  
(  
input  clk,  
input  rst_n,  
input  [7:0] datain_a,  
input  enable,  
output  reg [7:0] data_out  
);  
reg [7:0] data_out;  
always @(posedge clk) begin  	 	  
    if (rst_n == 1) begin    
        data_out <= '0;    
    end else begin    
        data_out <= datain_a[7:0];    
    end    
end    
endmodule    
```
### 正确输出应为

[输出]    
```verilog
module shift_reg  
(  
input clk,  
input rst_n,  
input 	[7:0] datain_a,  
input 	enable,  
output  [7:0] data_out  
);  	

always @(posedge clk or negedge) 	
begin	
  if(!rst_n)  	
	data_out <= 8'b0;    
  else if(enable)    
	data_out <= {datain_a[0],datain_a[7:1]};   
 end	
    
endmodule  
```
### 问题分析

重复定义了reg [7:0] data_out，通过prompt告诉他"I think data_out is already defined in the module definition and there is no need to repeat it."后，仍然重复定义该信号。


1.  always语句中的控制信号很容易出现混乱情况，包括组合逻辑中应该是"*"，出现"posedge+多种信号"。
2.  对于prompt中给出的信号，如enable信号，大模型给出的回答中缺少enable信号，即使在prompt中给出一个正确的示例试图纠正，让大模型再次生成对应问题的答案时，输出也不理想，prompt中有"answer according to the above example"等相关关键字。
4.  上下文理解能力有偏差，已经给出案例，并且详细说明，再次提问时采用一样的模板，提问内容不变，生成的答案与给出的案例有较大偏差



## enable信号缺失或定义错误

### 问题描述

在prompt中引入enable信号但在回答中可能出现缺失、定义出错等多种错误情况。

### Prompt示例
```
Q：
I am a digital circuit designer, I am trying to create a Verilog module named "ADD",it is a 8-bit adder module.It has"  
input 	wire[7:0]datain_a,  
input 	wire[7:0]datain_b,  
input 	wire enable,  
output  wire[7:0]data_out "  
enable==1 -> \data_out=datain_a+datain_b;  
Requirements:This module implements the addition of two 8bit numbers with enable signal.The enable signal determines whether the operation of addition is performed or not.  
Task:explain and write verilog code consistent with the provided information, use only synthesizable Verilog-2001 syntax.  

A:
Verilog-2001 syntax for the design of the add module:

module add(input [7:0] datain_a, input [7:0] datain_b, output [7:0] data_out);  
  assign data_out = datain_a + datain_b;  
endmodule  
```

### 正确输出应为
```
module ADD(  
input 	[7:0]datain_a,
input 	[7:0]datain_b,
input 	enable,
output  [7:0]data_out
);  
reg [7:0]dataout_temp;  
always @(*)  
  if(enable==1)  
	dataout_temp = datain_a + datain_b;  
assign data_out = dataout_temp;  
endmodule  
```

### 问题分析

缺少enable信号，通过one/few-shot方法纠正后，回答按给出的案例答案输出，但将提问与案例分开生成的情况依旧不理想。此外还存在enable信号在always控制信号中的情况，但enable一般不会出现在always控制信号中，enable的常用语句位	if（enable==1），在always块中。

## 生成代码不完整	

### 问题描述	

prompt长度并没有很长，但在让HDLGPT生成代码时，HDL并不生成完整的代码。

### Prompt示例	
[输入]
```verilog
Here is an example of the rom module:
"module rom(
    input wire clk,
    input wire rst,
    input wire we_i,                   
    input wire[31:0] addr_i,    
    input wire[31:0] data_i,
    output reg[31:0] data_o         
    );

    reg[31:0] rom[0:1024 - 1];

    always @ (posedge clk) begin
        if (we_i == 1) begin
            rom[addr_i] <= data_i;
        end
    end

    always @ (*) begin
        if (rst == 1) begin
            data_o = 32'b0;
        end else begin
            data_o = rom[addr_i];
        end
    end
endmodule"

I am trying to create a Verilog module called rom.The definition of the module is shown below:
"module rom(
    input wire clk,
    input wire rst,
    input wire we_i,                   
    input wire[31:0] addr_i,    
    input wire[31:0] data_i,
    output reg[31:0] data_o         
    );"
The rom contains 1024*32 bits of storage space. Writing data to rom is a temporal logic-data_i is written to memory at the address addr_i if the write enable we_i is 1. Read data from ROM is combinational logic, will address addr_i pointing to the data in memory of data_o assignment.Write verilog code that is consistent with the provided information.
```
[输出]
```verilog
module rom(
    input wire clk,
    input wire rst,
    input wire we_i,                   
    input wire[31:0] addr_i,    
    input wire[31:0] data_i,
    output reg[31:0] data_o         
    );
```
### 正确输出应为
[输出]
```verilog
module rom(
    input wire clk,
    input wire rst,
    input wire we_i,                   
    input wire[31:0] addr_i,    
    input wire[31:0] data_i,
    output reg[31:0] data_o         
    );

    reg[31:0] rom[0:1024 - 1];

    always @ (posedge clk) begin
        if (we_i == 1) begin
            rom[addr_i] <= data_i;
        end
    end

    always @ (*) begin
        if (rst == 1) begin
            data_o = 32'b0;
        end else begin
            data_o = rom[addr_i];
        end
    end
endmodule
```
### 问题分析

要求HDLGPT生成代码后，它仅生成了模块定义，一开始我觉得可能是prompt太长了，但随着prompt不断精简并没有解决这个问题。

## 相同输入下输出不稳定

### 问题描述

在同样的prompt输入下，多次重复输入大概率得到不同的回答，模型temperature参数过低，随机性过强

### 问题分析

希望能暂时将HDLGPT的temperature参数调高，降低生成结果随机性，尽量输出最高概率回答，方便prompt调试

## 模型似乎无法理解"例化"	

### 问题描述	

很难通过Prompt来让模型完成例化的操作。

### Prompt示例	
[输入]
```verilog
Here is the verilog code：
module SoC(
    input clk,
    input rst_n,
    output [7:0]DB,
    output [7:0]AB
    );
    wire RAM_en,RAM_read,RAM_write;
    CORE CORE_0 (
        .clk (clk),
        .rst_n (rst_n),
        .DB (DB),
        .AB (AB),
        .ROM_en (ROM_en),
        .ROM_read (ROM_read),
        .RAM_en (RAM_en),
        .RAM_read (RAM_read),
        .RAM_write (RAM_write)
    );

    RAM RAM_0 (
        .read (RAM_read),
        .write (RAM_write),
        .en (RAM_en),
        .data (DB),
        .addr (AB)
    );

endmodule

Now we need to instantiate a module called ROM:
module ROM(
    input read,
    input en,
    input [7:0]addr,
    output [7:0]data
    );

Please write verilog code to instantiate the ROM and any multiplexers that it needs that is consistent with the above definition.
```
[输出]
```verilog
Here is the Verilog code to instantiate the ROM:
module ROM(
input read,
input en,
input [7:0]addr,
output [7:0]data
);
```
### 正确输出应为
[输出]
```verilog
module SoC(
    input clk,
    input rst_n,
    output [7:0]DB,
    output [7:0]AB
    );
    wire ROM_en,ROM_read,RAM_en,RAM_read,RAM_write;
    CORE CORE_0 (
        .clk (clk),
        .rst_n (rst_n),
        .DB (DB),
        .AB (AB),
        .ROM_en (ROM_en),
        .ROM_read (ROM_read),
        .RAM_en (RAM_en),
        .RAM_read (RAM_read),
        .RAM_write (RAM_write)
    );

    RAM RAM_0 (
        .read (RAM_read),
        .write (RAM_write),
        .en (RAM_en),
        .data (DB),
        .addr (AB)
    );

    ROM ROM_0 (
        .read (ROM_read),
        .en (ROM_en),
        .addr (AB),
        .data (DB)
    );

endmodule
```
### 问题分析

让模型在SoC中例化模块ROM，但模型只输出ROM的模块定义。尝试精简Prompt直到Prompt只包含最基本的代码和指令后仍然只输出ROM的模块定义。

