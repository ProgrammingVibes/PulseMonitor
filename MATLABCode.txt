delete(instrfindall);
s = serial('com6'); %initialize serial communication
set(s,'BaudRate',115200); %speed at which we transmit (bits per second)
s.Terminator = 'CR'; % sets the terminator to CR/ new line
fopen(s);

grid on; %adds grid lines
j = 0;
data = 0;
t = 0;
tic ; % starts a stopwatch timer to measure performance

while (1) %loop 
    j = j + 1;
    tline = fgetl(s) %reads serial data and store it in an array
    data(j) = (str2double(tline))*(3/1024);
    t(j) = toc; %update clock, toc reads the elapsed time from the stopwatch timer 
    if j > 1
        xlim([t(j)-5 t(j)+5]); %update the x-axis
        line([t(j-1) t(j)],[data(j-1) data(j)]); %plot the data 
        xlabel('Time(s)'); %label the axis
        ylabel('Voltage(V)'); 
        hold on;
        drawnow %disp(data(j)); %originally was i
    end 
end
fclose(s); %close the ports used for serial communication
delete(s);
clear s;