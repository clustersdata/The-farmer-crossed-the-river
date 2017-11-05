# The-farmer-crossed-the-river

The farmer crossed the river

	一个农夫带着一只狼，一只羊和一些菜过河。河边只有一条一船，由
	于船太小，只能装下农夫和他的一样东西。在无人看管的情况下，狼要吃羊，羊
	要吃菜，请问农夫如何才能使三样东西平安过河。
  
【算法分析】

    将问题数字化。用１代表狼，２代表羊，３代表菜。则在河某一边物体的分布有以下
    
８种情况。

┏━━━━┯━┯━━━━━┯━━━━━━━━┯━━━┓

┃物体个数│０│    １	  │	   ２	    │	３  ┃
┠────┼─┼─┬─┬─┼──┬──┬──┼───┨

┃分布情况│０│１│２│３│1,2 │1,3 │2,3 │1,2,3 ┃
┠────┼─┼─┼─┼─┼──┼──┼──┼───┨

┃代码之和│０│１│２│３│３	│ ４ │ ５ │	６  ┃
┠────┼─┼─┼─┼─┼──┼──┼──┼───┨

┃是否相克│  │  │  │  │相克│    │相克│	    ┃
┗━━━━┷━┷━┷━┷━┷━━┷━━┷━━┷━━━┛

当（两物体在一起而且）代码和为３或５时，必然是相克物体在一起的情况。

【参考程序】

const

     wt:array[0..3]of string[5]=('     ', 'WOLF ','SHEEP','LEAVE');
     
var left,right:array[1..3] of integer ;

    what,i,total,left_rest,right_rest:integer;
    
procedure print_left; {输出左岸的物体}

var i:integer;

begin

     total:=total+1;
     
     write('(',total,')');  {第几次渡河}
     
     for i:=1 to 3 do	write(wt[left[i]]);
     
     write('|',' ':4);
     
end;

procedure print_right;{输出右岸的物体}

var i:integer;

begin

     write(' ':4,'|');
     
     for i:=1 to 3 do if right[i]<>0 then write(wt[right[i]]);
     
     writeln;
     
end;

procedure print_back(who:integer);  {右岸矛盾时，需从右岸捎物体→左岸}

var i:integer;

begin

     for i:=1 to 3 do begin
     
	 if not ((i=who) or (right[i]=0)) then begin
   
	 {要捎回左岸的物体不会时刚刚从左岸带来的物体，也不会是不在右岸的物体}
   
	       what:=right[i];
         
	       right[i]:=0;
         
	 print_left;  {输出返回过程}
   
	 write('<-',wt[i]);
   
	 print_right;
   
	 left[i]:=what;  {物体到达左岸}
   
	 end;
   
     end;
     
end;

begin

     total:=0;
     
     for i:=1 to 3 do begin  left[i]:=i; right[i]:=0;end;
     
     repeat
     
       for i:=1 to 3 do    {共有３种物体}
       
	 if left[i]<>0 then  {第Ｉ种物体在左岸}
   
	  begin
    
	    what:=left[i];left[i]:=0;	{what:放置将要过河的物体编号}
      
	    left_rest:=left[1]+left[2]+left[3];  {求左岸剩余的物体编号总和}
      
	    if (left_rest=3) or (left_rest=5) then left[i]:=what
      
      
			{假如左岸矛盾，则不能带第Ｉ种过河，尝试下一物体}
      
	       else	{否则可带过河}
         
         
		    begin print_left;	 {输出过河过程}
        
			  write('->',wt[i]);
        
			  print_right;
        
        
			  right[i]:=what;  {物体到达右岸}
        
			  if left_rest=0 then halt;  {左岸物体已悉数过河}
        
			  right_rest:=right[1]+right[2]+right[3];
        
					      {求右岸剩余的物体编号总和}
                
                
			 if (right_rest=3)or(right_rest=5) then print_back(i)
       
					       {右岸有矛盾，要捎物体回左岸}
                 
			     else begin print_left;  {右岸有矛盾，空手回左岸}
           
					write('<-',' ':5);
          
					print_right;
          
				  end;
          
		    end;
        
	  end;
    
    
       until false;  {不断往返}
       
end.
