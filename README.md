#include <iostream>
#include <vector>
#include <iomanip>
#include <limits>

using namespace std;

const int DAYS = 7;
const int READINGS = 4;

void inputTemperatures(vector<vector<double>>& temps);
void findExtremes(const vector<vector<double>>& temps, double& maxTemp, double& minTemp);
vector<double> calculateDailyAverages(const vector<vector<double>>& temps);
void displayTable(const vector<vector<double>>& temps, const vector<double>& avgs, double maxTemp, double minTemp);

int main() {
    vector<vector<double>> temperatureData(DAYS, vector<double>(READINGS));
    double maxTemp, minTemp;
    vector<double> dailyAverages;

    inputTemperatures(temperatureData);
    findExtremes(temperatureData, maxTemp, minTemp);
    dailyAverages = calculateDailyAverages(temperatureData);
    displayTable(temperatureData, dailyAverages, maxTemp, minTemp);

    return 0;
}

void inputTemperatures(vector<vector<double>>& temps) {
    for (int i = 0; i < DAYS; ++i) {
        cout << "Enter 4 readings for Day " << (i + 1) << ": ";
        for (int j = 0; j < READINGS; ++j) {
            cin >> temps[i][j];
        }
    }
    cout << endl;
}

void findExtremes(const vector<vector<double>>& temps, double& maxTemp, double& minTemp) {
    maxTemp = temps[0][0];
    minTemp = temps[0][0];

    for (const auto& day : temps) {
        for (double temp : day) {
            if (temp > maxTemp) {
                maxTemp = temp;
            }
            if (temp < minTemp) {
                minTemp = temp;
            }
        }
    }
}

vector<double> calculateDailyAverages(const vector<vector<double>>& temps) {
    vector<double> averages;
    
    for (const auto& day : temps) {
        double sum = 0;
        for (double temp : day) {
            sum += temp;
        }
        averages.push_back(sum / READINGS);
    }
    return averages;
}

void displayTable(const vector<vector<double>>& temps, const vector<double>& avgs, double maxTemp, double minTemp) {
    cout << "---------------------------------------------------------" << endl;
    cout << "                  WEEKLY TEMPERATURE REPORT              " << endl;
    cout << "---------------------------------------------------------" << endl;
    
    cout << left << setw(10) << "Day" 
         << setw(10) << "Read 1" 
         << setw(10) << "Read 2" 
         << setw(10) << "Read 3" 
         << setw(10) << "Read 4" 
         << setw(10) << "Daily Avg" << endl;
    
    cout << "---------------------------------------------------------" << endl;

    for (int i = 0; i < DAYS; ++i) {
        cout << left << setw(10) << ("Day " + to_string(i + 1)); 
        
        for (int j = 0; j < READINGS; ++j) {
            cout << fixed << setprecision(1) << setw(10) << temps[i][j];
        }
        
        cout << fixed << setprecision(2) << setw(10) << avgs[i] << endl;
    }

    cout << "---------------------------------------------------------" << endl;
    cout << "Highest Temperature of the Week: " << maxTemp << endl;
    cout << "Lowest Temperature of the Week:  " << minTemp << endl;
    cout << "---------------------------------------------------------" << endl;
}
