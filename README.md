#include <iostream>
#include <fstream>
#include <vector>
#include <cmath>
#include <stdexcept>
// Функція для зчитування даних з файлу та побудови таблиці
std::vector<std::vector<double>> readTableFromFile(const std::string& filename) {
    std::ifstream file(filename);
    if (!file.is_open()) {
        throw std::runtime_error("Помилка відкриття файлу " + filename);
    }
    std::vector<std::vector<double>> table;
    double x, U, T;
    std::string text;
    while (file >> x >> U >> T >> text) {
        table.push_back({x, U, T});
    }
    return table;
}
// Функція для обчислення максимального значення серед параметрів
double computeMax(const std::vector<double>& params) {
    double maxVal = params[0];
    for (size_t i = 1; i < params.size(); ++i) {
        if (params[i] > maxVal) {
            maxVal = params[i];
        }
    }
    return maxVal;
}
// Алгоритм 1
double algorithm1(double z, double y, double x) {
    return z * y * x;
}
// Алгоритм 2
double algorithm2(double z, double y, double x) {
    return y * z * (x - y);
}
// Алгоритм 3
double algorithm3(double z, double y, double x) {
    return z * y * (x + y);
}
// Алгоритм 4
double algorithm4(double z, double y, double x) {
    return (y * x - 0.5) * (x * x);
}
int main() {
    double x, y, z;
    std::string text;
    std::cout << "Введіть значення x, y, z та текст: ";
    std::cin >> x >> y >> z >> text;
    try {
        std::vector<std::vector<double>> table4 = readTableFromFile("dat1.dat");
        std::vector<std::vector<double>> table5 = readTableFromFile("dat2.dat");
        std::vector<std::vector<double>> table6 = readTableFromFile("dat3.dat");
        double result;
        if (text.empty()) {
            result = computeMax({x, y, z});
        } else {
            result = std::stod(text);
        }
        if (x <= 5) {
            result = algorithm3(z, y, x);
        } else if (x > 5 && x <= 10) {
            result = algorithm4(z, y, x);
        }
        std::cout << "Результат обчислення func(u,v, text): " << result << std::endl;
    } catch (const std::runtime_error& e) {
        std::cerr << "Помилка: " << e.what() << std::endl;
    }
    return 0;
}
