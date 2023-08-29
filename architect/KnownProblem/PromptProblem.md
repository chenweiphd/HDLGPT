1.  always语句中的控制信号很容易出现混乱情况，包括组合逻辑中应该是"*"，出现"posedge+多种信号"。
2.  对于prompt中给出的信号，如enable信号，大模型给出的回答中缺少enable信号，即使在prompt中给出一个正确的示例试图纠正，让大模型再次生成对应问题的答案时，输出也不理想，prompt中有"answer according to the above example"等相关关键字。
