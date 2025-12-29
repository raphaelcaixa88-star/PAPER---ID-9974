clear all;
close all;
clc;
%Signal characterization
y1=load('Awake.txt'); %Load signal from awake rat
fs = 500; % find the sampling rate or frequency
T = 1/fs;% sampling rate or frequency
% find the length of the data per second
N = length(y1);
ls = size(y1);
t = (0 : N-1)/fs;% sampling period


%%%%%%%% Low-pass and High-pass filtering 

[bl,al]=butter(10,10.0/250);
y11=filtfilt(bl,al,y1);%low-pass
[bh,ah]=butter(4,0.5/250,'high');
y12=filtfilt(bh,ah,y11);%high-pass

%Filter notch / 60 Hz

d = designfilt('bandstopiir','FilterOrder',4, ...
               'HalfPowerFrequency1',58,'HalfPowerFrequency2',61, ...
               'DesignMethod','butter','SampleRate',fs);
           buttLoop =filtfilt(d,y12);
           
%Baseline wander removal (polynomial detrending)  

[p,s,mu] = polyfit((1:numel(buttLoop))',buttLoop,6);
f_y = polyval(p,(1:numel(buttLoop))',[],mu);

ECG_data = buttLoop - f_y;        % Detrend data

pp22=fft(ECG_data);
xf=linspace(0,100,length(ECG_data));

% Visualizationf of FFT Graphic
figure(1)
plot(xf,abs(pp22),'black')
xlabel('Frequency (Hz)'); ylabel('Amplitude (V)')
% axis([0 3.5]); Establish two equal frequency for both images anesthetized and awake.
