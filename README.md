# SkinCancerTrends

%BME3053C Final Project


clc; clear;

data = readtable("Melanoma_Data.csv");

rate = table2array(data);


% Graph with Outliers

subplot(1,2,1)

x = rate(:, 1);

y = rate(:, 2);


scatter(x, y, "*");

xlabel('Latitude (°)');

ylabel('Melanoma Incidence Rate Per 100,000, 2021');

title('Melanoma Incidences by Latitude of Floridian Cities (With Outliers)');


%Line of best fit

p = polyfit(x, y, 1);

x_fit = min(x):0.1:max(x);

y_fit = polyval(p, x_fit);

hold on;

plot(x_fit, y_fit, 'r');

legend('Data', 'Line of Best Fit');

hold off;


%Filter out outliers

indices_to_remove = [];

for ii = 1:size(rate, 1)

    if rate(ii, 2) > quantile(rate(:, 2), 0.75) + 1.5 * iqr(rate(:, 2))
    
        indices_to_remove = [indices_to_remove, ii];
        
    elseif rate(ii, 2) < quantile(rate(:, 2), 0.25) - 1.5 * iqr(rate(:, 2))
    
        indices_to_remove = [indices_to_remove, ii];
        
    end
    
end


rate(indices_to_remove, :) = [];


% Graph without Outliers

subplot(1,2,2)

x = rate(:, 1);

y = rate(:, 2);


scatter(x, y, "*");

xlabel('Latitude (°)');

ylabel('Melanoma Incidence Rate Per 100,000, 2021');

title('Melanoma Incidences by Latitude of Floridian Cities (Without Outliers)');


%Line of best fit

p = polyfit(x, y, 1);

x_fit = min(x):0.1:max(x);

y_fit = polyval(p, x_fit);

hold on;

plot(x_fit, y_fit, 'r');

legend('Data', 'Line of Best Fit');

hold off;


%Linear Equation: y = -2.722x +100.2

%R^2 Value = 0.6857

Lat = input('Enter latitude between 24° and 31°: ');

Incidences = -2.722*Lat + 100.2;

fprintf('The estimated rate of melanoma incidences at a latitude of %g° is %g per 100,000 people\n',Lat,Incidences);
