//autor: Alan Montaño

#include <iostream>
#include <vector>
#include <numeric>
#include <iomanip>
#include <string>

struct LinearRegression {
    double beta_0; // Intercepto (B_0)
    double beta_1; // Pendiente  (B_1)
};

LinearRegression calculateLinearRegression(const std::vector<double>& x, const std::vector<double>& y) {
    size_t n = x.size();
    
    double sum_x = 0.0, sum_y = 0.0;
    double sum_xy = 0.0, sum_x2 = 0.0;

    for (size_t i = 0; i < n; ++i) {
        sum_x += x[i];
        sum_y += y[i];
        sum_xy += x[i] * y[i];
        sum_x2 += x[i] * x[i];
    }

    double beta_1 = (n * sum_xy - sum_x * sum_y) / (n * sum_x2 - sum_x * sum_x);

    double beta_0 = (sum_y - beta_1 * sum_x) / n;

    return {beta_0, beta_1};
}

double predict(double x, double beta_0, double beta_1) {
    return beta_0 + beta_1 * x;
}

int main() {
    std::cout << std::fixed << std::setprecision(4);

    std::vector<double> advertising = {43.60, 50.44, 59.01, 66.30, 82.36, 92.15, 100.51, 110.06, 111.51};
    std::vector<double> sales       = {1261.08, 1475.28, 1657.52, 2059.05, 2303.76, 2512.64, 2751.46, 2787.67, 2939.13};

    LinearRegression model = calculateLinearRegression(advertising, sales);

    std::cout << "========================================================\n";
    std::cout << "            REGRESION LINEAL - CASO BENETTON             \n";
    std::cout << "========================================================\n\n";

    std::cout << "Coeficientes Calculados:\n";
    std::cout << "  B_0 (Intercepto) = " << model.beta_0 << "\n";
    std::cout << "  B_1 (Pendiente)  = " << model.beta_1 << "\n\n";

    std::string signo = (model.beta_1 >= 0) ? " + " : " - ";
    std::string ecuacion = "Sales = " + std::to_string(model.beta_0) + signo + std::to_string(std::abs(model.beta_1)) + " * Advertising";
    
    std::cout << "--------------------------------------------------------\n";
    std::cout << "Ecuacion de Regresion Calculada:\n";
    std::cout << "  " << ecuacion << "\n";
    std::cout << "--------------------------------------------------------\n\n";

    std::vector<double> nuevos_advertising = {55.00, 75.00, 95.00, 105.00, 120.00};

    std::cout << "Predicciones de Sales para 5 nuevos valores de Advertising:\n";
    std::cout << "--------------------------------------------------------\n";
    std::cout << std::setw(15) << "Advertising (X)" << " | " << std::setw(15) << "Sales (Y Pred)" << "\n";
    std::cout << "--------------------------------------------------------\n";

    for (double x_val : nuevos_advertising) {
        double y_pred = predict(x_val, model.beta_0, model.beta_1);
        std::cout << std::setw(15) << x_val << " | " << std::setw(15) << y_pred << "\n";
    }
    
    std::cout << "--------------------------------------------------------\n";

    return 0;
}
