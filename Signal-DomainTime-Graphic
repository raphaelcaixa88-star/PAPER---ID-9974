clear; 
close all; 
clc;

%% Load ECG signals
y_anesth = load('Anesthetized.txt'); %Load signal from anesthetized rat
y_awake  = load('Awake.txt'); %Load signal from awake rat

fs = 500;                 % Sampling frequency (Hz)
t1 = (0:length(y_anesth)-1)/fs;
t2 = (0:length(y_awake)-1)/fs;

%% Band-pass filtering (QRS-focused)
[b_lp,a_lp] = butter(4,35/(fs/2),'low');
[b_hp,a_hp] = butter(4,18/(fs/2),'high');

yA = filtfilt(b_hp,a_hp,filtfilt(b_lp,a_lp,y_anesth));
yW = filtfilt(b_hp,a_hp,filtfilt(b_lp,a_lp,y_awake));

%% 60 Hz notch filter
notch = designfilt('bandstopiir','FilterOrder',2, ...
                   'HalfPowerFrequency1',58, ...
                   'HalfPowerFrequency2',61, ...
                   'DesignMethod','butter', ...
                   'SampleRate',fs);

yA = filtfilt(notch,yA);
yW = filtfilt(notch,yW);

%% Baseline wander removal (polynomial detrending)
[pA,~,muA] = polyfit((1:numel(yA))',yA,6);
baselineA  = polyval(pA,(1:numel(yA))',[],muA);
ECG_A      = yA - baselineA;

[pW,~,muW] = polyfit((1:numel(yW))',yW,6);
baselineW  = polyval(pW,(1:numel(yW))',[],muW);
ECG_W      = yW - baselineW;

%% Visualization
figure
subplot(2,1,1)
plot(t1,ECG_A)
title('Anesthetized')
axis([]); % Establish two equal times for both images.

subplot(2,1,2)
plot(t2,ECG_W)
title('Awake')
axis([]); % Establish two equal times for both images.


subplot(2,1,2)
plot(t,y1)% grid
legend('Awake')
axis([]); % Establish two equal times for both images.
