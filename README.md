# laba2
{Kunitsa Victoria 2 kurs, 3 gruppa 5 variant}
Uses Graph, crt;
Type
   TFunc = Function (x : real) : real;
Var c,c2:char;
    dx, dy, v : integer;
    gd, gm : integer;
    x1, y1, x2, y2, ColorGraph, ColorAxis : integer;
    FiFirst, FiLast, dfi : real;

{$F+}
Function f1 (fi : real) : real;
  begin
        f1 := sin (sqrt(2)*fi);
  end;
Function f2 (fi : real) : real;
  begin
        f2 := sin(4/ 3*fi);
  end;
Function f3 (fi : real) : real;
  begin
        f3 := sin (3 * fi);
  end;
Function f4 (fi : real) : real;
  begin
        f4 := sin (5/ 4*fi);
  end;
{$F-}
Procedure PolarToCurtesian (fi, r : real;  Var x, y : real);
  begin
        x := r*cos(fi);
        y := r*sin(fi);
  end;
Procedure FindMaxMinXY (f : TFunc; Var MaxX, MinX, MaxY, MinY : real; FiFirst, FiLast, dfi : real);
 Var x, y, fi : real;
  begin
      fi := FiFirst;
      PolarToCurtesian (fi, f1(fi), x, y);
      MaxX := x;
      MinX := x;
      MaxY := y;
      MinX := y;
      fi := fi + dfi;
        while fi <= FiLast + dfi/10 do
          begin
            PolarToCurtesian (fi, f1(fi), x, y);
            if x > MaxX then MaxX := x
            else if x < MinX then MinX := x;
            if y > MaxY then MaxY := y
            else if y < MinY then MinY := y;
            fi := fi + dfi;
          end;
  end;
Function Min (a, b : real) : real;
  begin
   if a < b then Min := a
   else Min := b;
  end;
Function Max (a, b : real) : real;
  begin
   if a > b then Max := a
   else Max := b;
  end;
Procedure GetMashtab (x1, y1, x2, y2 : integer; dx, dy : integer; MaxX, MaxY, MinX, MinY : real; Var Mash : real);
  Var
    LX, LY : integer;
    MashX, MashY : real;
   begin
     LX := (x2 - x1 - 8*dx) div 2;
     LY := (y2 - y1 - 8*dy) div 2;
     MashX := LX / Max(abs(MaxX), abs(MinX));
     MashY := LY / Max(abs(MaxY), abs(MinY));
     Mash := Min (MashX, MashY);
   end;
Procedure PlotAxis (x1, y1, x2, y2, dx, dy : integer; ColorAxis : integer);
  begin
    SetColor (ColorAxis);
    Line (x1 + dx, y1 + (y2 - y1) div 2, x2 - dx, y1 + (y2 - y1) div 2);
    Line (x1 + (x2 - x1) div 2, y1 + dy, x1 + (x2 - x1) div 2, y2 - dy);
    Line (x2 - 2*dx, y1 + (y2 - y1) div 2 - dy, x2 - dx, y1 + (y2 - y1) div 2);
    Line (x2 - 2*dx, y1 + (y2 - y1) div 2 + dy, x2 - dx, y1 + (y2 - y1) div 2);
    Line (x1 + (x2 - x1) div 2 - dx, y1 + 2*dy, x1 +(x2 - x1) div 2, y1 + dx);
    Line (x1 + (x2 - x1) div 2 + dx, y1 + 2*dy, x1 +(x2 - x1) div 2, y1 + dx);
    OutTextXY (x1 + (x2 - x1 + 20) div 2, y1 + dy, 'y');
    OutTextXY (x2 - dx-10, y1 + (y2 - y1 + 10) div 2+5, 'x');
  end;
Procedure DrawGraph (x1, y1, x2, y2, dx, dy : integer; ColorGraph : integer; f : TFunc; FiFirst, FiLast, dfi : real);
  Var MaxX, MaxY, MinX, MinY : real;
      Mash : real; fi, r, x, y : real; xg, yg : integer;
   begin
     FindMaxMinXY (f, MaxX, MinX, MaxY, MinY, FiFirst, FiLast, dfi);
     GetMashtab (x1, y1, x2, y2, dx, dy, MaxX, MinX, MaxY, MinY, Mash);
     fi := FiFirst;
     while fi <= FiLast + dfi/10 do
        begin
          r := f(fi);
          PolarToCurtesian (fi, r, x, y);
          xg := Round(x * Mash) + (x2 - x1) div 2 + x1;
          yg := Round(y * Mash) + (y2 - y1) div 2 + y1;
          PutPixel (xg, yg, ColorGraph);
          fi := fi + dfi;
        end;
   end;
BEGIN
 gd := Detect;
 InitGraph (gd, gm, '');
 dfi := 0.001;
 dx := 8;
 dy := 8;
 repeat
   ClearDevice;
   x1 := 0;
   y1 := 0;
   x2 := GetMaxX div 2;
   y2 := GetMaxY div 2;
   FiFirst := 0;
   FiLast := 50*Pi;
   ColorAxis := white;
   ColorGraph := red;
   SetViewPort (x1, y1, x2, y2, ClipOn);
   x2 := x2 - x1;
   y2 := y2 - y1;
   x1 := 0;
   y1 := 0;
   PlotAxis (x1, y1, x2, y2, dx, dy, ColorAxis);
   DrawGraph (x1, y1, x2, y2, dx, dy, ColorGraph, @f1, FiFirst, FiLast, dfi);
   OutTextXY (10,10,'sin (sqrt(2)*fi)');

   x1 := 0;
   y1 := GetMaxY div 2;
   x2 := GetMaxX div 2;
   y2 := GetMaxY;
   FiFirst := 0;
   FiLast := 10*Pi;
   ColorAxis := white;
   ColorGraph := red;
   SetViewPort (x1, y1, x2, y2, ClipOn);
   x2 := x2 - x1;
   y2 := y2 - y1;
   x1 := 0;
   y1 := 0;
   PlotAxis (x1, y1, x2, y2, dx, dy, ColorAxis);
   DrawGraph (x1, y1, x2, y2, dx, dy, ColorGraph, @f2, FiFirst, FiLast, dfi);
   OutTextXY (10,10,'sin (4/ 3*fi)');

   x1 := GetMaxX div 2;
   y1 := 0;
   x2 := GetMaxX;
   y2 := GetMaxY div 2;
   FiFirst := 0;
   FiLast := 5*Pi;
   ColorAxis := white;
   ColorGraph := red;
   SetViewPort (x1, y1, x2, y2, ClipOn);
   x2 := x2 - x1;
   y2 := y2 - y1;
   x1 := 0;
   y1 := 0;
   PlotAxis (x1, y1, x2, y2, dx, dy, ColorAxis);
   DrawGraph (x1, y1, x2, y2, dx, dy, ColorGraph, @f3, FiFirst, FiLast, dfi);
   OutTextXY (10,10,'sin (3 * fi)');

   x1 := GetMaxX div 2;
   y1 := GetMaxY div 2;
   x2 := GetMaxX;
   y2 := GetMaxY;
   FiFirst := 0;
   FiLast := 10*Pi;
   ColorAxis := white;
   ColorGraph := red;
   SetViewPort (x1, y1, x2, y2, ClipOn);
   x2 := x2 - x1;
   y2 := y2 - y1;
   x1 := 0;
   y1 := 0;
   PlotAxis (x1, y1, x2, y2, dx, dy, ColorAxis);
   DrawGraph (x1, y1, x2, y2, dx, dy, ColorGraph, @f4, FiFirst, FiLast, dfi);
   OutTextXY (10,10,'sin (5/ 4*fi)');
   SetViewPort (0, 0, GetMaxX, GetMaxY, ClipOn);

     repeat
     c:=readkey;
       case c of
         #77: begin
                v:=v+1;
              end;
         #75: begin
                v:=v-1;
              end;
         #72: begin
                v:=v-2;
              end;
         #80:  begin
                v:=v+2;
               end;
       end;
       case (v mod 4) of
            1: begin
                 SetColor(15);
                 rectangle(5,5,(GetMaxX div 2 )-5, (GetMaxY div 2)-5);
                 SetColor(0);
                 rectangle((GetMaxX div 2)+5,5,GetMaxX -5, (GetMaxY div 2)-5);
                 rectangle(5,(GetMaxY div 2)+5,(GetMaxX div 2)-5 , GetMaxY-5 );
                 rectangle((GetMaxX div 2)+5 ,(GetMaxY div 2)+5,GetMaxX-5 , GetMaxY -5);
               end;
            2: begin
                 SetColor(15);
                 rectangle((GetMaxX div 2)+5,5,GetMaxX -5, (GetMaxY div 2)-5);
                 SetColor(0);
                 rectangle(5,5,(GetMaxX div 2 )-5, (GetMaxY div 2)-5);
                 rectangle(5,(GetMaxY div 2)+5,(GetMaxX div 2)-5 , GetMaxY-5 );
                 rectangle((GetMaxX div 2)+5 ,(GetMaxY div 2)+5,GetMaxX-5 , GetMaxY -5);
               end;
            3: begin
                 SetColor(15);
                 rectangle(5,(GetMaxY div 2)+5,(GetMaxX div 2)-5 , GetMaxY-5 );
                 SetColor(0);
                 rectangle((GetMaxX div 2)+5,5,GetMaxX -5, (GetMaxY div 2)-5);
                 rectangle(5,5,(GetMaxX div 2 )-5, (GetMaxY div 2)-5);
                 rectangle((GetMaxX div 2)+5 ,(GetMaxY div 2)+5,GetMaxX-5 , GetMaxY -5);
               end;
            0: begin
                 SetColor(15);
                 rectangle((GetMaxX div 2)+5 ,(GetMaxY div 2)+5,GetMaxX-5 , GetMaxY -5);
                 SetColor(0);
                 rectangle(5,(GetMaxY div 2)+5,(GetMaxX div 2)-5 , GetMaxY-5 );
                 rectangle((GetMaxX div 2)+5,5,GetMaxX -5, (GetMaxY div 2)-5);
                 rectangle(5,5,(GetMaxX div 2 )-5, (GetMaxY div 2)-5);
               end;
         end;
    if v=0 then v:=120;
    if v=240 then v:=120;
    UNTIL c=#71;

    case (v mod 4) of
      1: begin
          FiLast:=50*pi;
          ClearDevice;
          SetColor(15);
          OutTextXY (10,10,'sin (sqrt(2)*fi)');
          SetViewPort (0, 0, GetMaxX, GetMaxY, ClipOn);
          PlotAxis (0, 0, GetMaxX,GetMaxY, dx, dy, ColorAxis);
          DrawGraph (0, 0, GetMaxX,GetMaxY, dx, dy, ColorGraph, @f1, FiFirst, FiLast, dfi);
        end;
     2: begin
          FiLast := 5*Pi;
          ClearDevice;
          SetColor(15);
          OutTextXY (10,10,'sin (3 * fi)');
          SetViewPort (0, 0, GetMaxX, GetMaxY, ClipOn);
          PlotAxis (0, 0, GetMaxX ,GetMaxY, dx, dy, ColorAxis);
          DrawGraph (0, 0, GetMaxX,GetMaxY, dx, dy, ColorGraph, @f3, FiFirst, FiLast, dfi);
        end;
     3: begin
          FiLast := 10*Pi;
          ClearDevice;
          SetColor(15);
          OutTextXY (10,10,'sin (4/ 3*fi)');
          SetViewPort (0, 0, GetMaxX, GetMaxY, ClipOn);
          PlotAxis (0, 0, GetMaxX ,GetMaxY, dx, dy, ColorAxis);
          DrawGraph (0, 0, GetMaxX,GetMaxY, dx, dy, ColorGraph, @f2, FiFirst, FiLast, dfi);
        end;
     0: begin
          FiLast := 10*Pi;
          ClearDevice;
          SetColor(15);
          OutTextXY (10,10,'sin (5/ 4*fi)');
          SetViewPort (0, 0, GetMaxX, GetMaxY, ClipOn);
          PlotAxis (0, 0, GetMaxX,GetMaxY, dx, dy, ColorAxis);
          DrawGraph (0, 0, GetMaxX ,GetMaxY, dx, dy, ColorGraph, @f4, FiFirst, FiLast, dfi);
        end;
     end;
  while not(c=#27) do begin c:= readkey end;
 until c=#9;
 readln;
end.
