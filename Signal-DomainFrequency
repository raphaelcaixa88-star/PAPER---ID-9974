clear all;
close all;
clc;
%Caracterização do sinal
y1=load('.txt');
fs = 500; % find the sampling rate or frequency
T = 1/fs;% sampling rate or frequency
% find the length of the data per second
N = length(y1);
ls = size(y1);
t = (0 : N-1)/fs;% sampling period


%%%%%%%% FILTROS PASSA BAIXA E PASSA ALTA

[bl,al]=butter(10,10.0/250);
y11=filtfilt(bl,al,y1);%low-pass
[bh,ah]=butter(4,0.5/250,'high');
y12=filtfilt(bh,ah,y11);%high-pass

%%%%%%%% FILTRO NOT///CH 60HZ

d = designfilt('bandstopiir','FilterOrder',4, ...
               'HalfPowerFrequency1',58,'HalfPowerFrequency2',61, ...
               'DesignMethod','butter','SampleRate',fs);
           buttLoop =filtfilt(d,y12);
           
%%%%%%%% LINHA DE BASE DO SINAL          

[p,s,mu] = polyfit((1:numel(buttLoop))',buttLoop,6);
f_y = polyval(p,(1:numel(buttLoop))',[],mu);

ECG_data = buttLoop - f_y;        % Detrend data

pp22=fft(ECG_data);
xf=linspace(0,100,length(ECG_data));

figure(1)
%plot((tths),'b-')
plot(xf,abs(pp22),'black')
xlabel('Frequency (Hz)'); ylabel('Amplitude (V)')
% %axis([34.45 34.95 3.9 4.3]);
% %axis([15 16.7 -0.5 0.5]);
