# Group-7---U22CS1070
--- | ---
GROUP MEMBERS 

U22CS1064
ahmad-ig
https://github.com/ahmad-ig/Simplex-Algorithm

U22cs1065 
thebuhariii 
https://github.com/thebuhariii/Group-7---simplex-algorithm-

U22CS1070
UmarIssa
https://github.com/UmarIssa/Group-7---U22CS1070

U22CS1062 
Maryam876
https://github.com/Maryam876/Group-7--simplex-algorithm-

U22CS1069
Big-Major
https://github.com/Big-Major/Group-7---Simplex-algorithm-

U22CS1068
jerry
https://github.com/U22cs1068/Group-7-Simplex-Algorithm-

---

This program lets you input the number of variables and constraints, followed by the coefficients of the constraints (including the right-hand side) and the objective function. It then performs the Simplex Algorithm to find the optimal solution.
Purpose of the Code
This program implements the Simplex Algorithm to solve a linear programming problem. It finds the maximum value of an objective function.

---

1. Input Handling
--
Code snippet
int numVariables, numConstraints;
cout << "Enter the number of variables: ";
cin >> numVariables;
cout << "Enter the number of constraints: ";
cin >> numConstraints;
• The user specifies the number of decision variables and constraints.
• Example: For x1 and x2 , i.e numVariables = 2.


3. Simplex Table Construction
Code snippet
vector<vector<double>> table(numConstraints + 1, vector<double>(numVariables + numConstraints + 1, 0));
 
This creates a 2D matrix (Simplex Table) to store:
• Coefficients of decision variables x1,x2,…
• Slack variables s1,s2,…
• The right-hand side (RHS) of constraints.
 
 
 
4. Fill the Table
Code snippet
for (int i = 0; i < numConstraints; i++) {
   for (int j = 0; j <= numVariables; j++) {
       cin >> table[i][j];
   }
   table[i][numVariables + i + 1] = 1; // Slack variable
}
 
• Input the constraints coefficients and RHS values.
• Adds slack variables (1 in their own column, 0 elsewhere) to convert inequalities (≤) into equations.
Code snippet
for (int j = 1; j <= numVariables; j++) {
   cin >> table.back()[j];
   table.back()[j] *= -1; // Convert to maximization form
}
• Input the objective function coefficients and store them in the last row (negated to suit the Simplex method).
 
5. Printing the Table
Code snippet
void printTable(const vector<vector<double>>& table) {
   for (const auto& row : table) {
       for (double value : row) {
           cout << setw(10) << value << " ";
       }
       cout << endl;
   }
}
 
• Prints the simplex table neatly for debugging or understanding each step.
 
 
 
 
6. Finding the Pivot Column
Code Snippet
int findPivotColumn(const vector<vector<double>>& table) {
   int pivotColumn = -1;
   double minValue = 0;
   for (int j = 1; j < table[0].size(); j++) {
       if (table.back()[j] < minValue) {
           minValue = table.back()[j];
           pivotColumn = j;
       }
   }
   return pivotColumn;
}
• The pivot column is the one with the most negative value in the last row (objective function). This indicates which variable can improve the solution.
 
7. Finding the Pivot Row
Code snippet
int findPivotRow(const vector<vector<double>>& table, intpivotColumn) {
   int pivotRow = -1;
   double minRatio = 1e9; // Large value for comparison
   for (int i = 0; i < table.size() - 1; i++) {
       if (table[i][pivotColumn] > 0) {
           double ratio = table[i][0] / table[i][pivotColumn];
           if (ratio < minRatio) {
               minRatio = ratio;
               pivotRow = i;
           }
       }
   }
   return pivotRow;
}
 
• The pivot row is determined by the minimum ratio test: RHS/Pivot Column Coefficient 
• This ensures feasibility and avoids unbounded solutions.
 
 
 
 
 
8. Performing Pivoting
Code snippet
void performPivoting(vector<vector<double>>& table, intpivotRow, int pivotColumn) {
   double pivotValue = table[pivotRow][pivotColumn];
   for (double& value : table[pivotRow]) {
       value /= pivotValue; // Normalize pivot row
   }
 
   for (int i = 0; i < table.size(); i++) {
       if (i != pivotRow) {
           double factor = table[i][pivotColumn];
           for (int j = 0; j < table[i].size(); j++) {
               table[i][j] -= factor * table[pivotRow][j]; // Eliminate column values
           }
       }
   }
}
• This step normalizes the pivot row and adjusts the other rows to make the pivot column a unit column (Gaussian elimination).
9. Simplex Iterations
Code snippet
void simplex(vector<vector<double>>& table) {
   while (true) {
       int pivotColumn = findPivotColumn(table);
       if (pivotColumn == -1) {
           break; // Optimal solution found
       }
       int pivotRow = findPivotRow(table, pivotColumn);
       if (pivotRow == -1) {
           cout << "Unbounded solution." << endl;
           return;
       }
       performPivoting(table, pivotRow, pivotColumn);
       cout << "Table after pivoting (Row: " << pivotRow << ", Column: " << pivotColumn << "):\\n";
       printTable(table);
   }
 
   // Print results
   cout << "Optimal solution found:\\n";
   for (int i = 1; i < table[0].size(); i++) {
       cout << "x" << i << " = ";
       bool found = false;
       for (int j = 0; j < table.size() - 1; j++) {
           if (table[j][i] == 1) {
               cout << table[j][0] << " ";
               found = true;
               break;
           }
       }
       if (!found) {
           cout << "0 ";
       }
   }
   cout << endl;
   cout << "Maximum value: " << table.back()[0] << endl;
}
• The algorithm iteratively:
1. Finds the pivot column and row.
2. Performs pivoting.
3. Stops when there are no negative values in the last row (optimal solution).
 
 
 
 
 
 
9. Main Function
Code snippet
int main() {
   // Get problem dimensions and inputs
   // Build simplex table
   // Run the simplex algorithm
}
 
• Brings everything together:
1. Input the problem's data.
2. Construct the simplex table.
3. Call the simplex function to solve.
  

