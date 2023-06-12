# Kursach
using System;

class MatrixCalculator
{
    static void Main()
    {
        int choice;
        do
        {
            Console.WriteLine("Меню:");
            Console.WriteLine("1. Додавання матриць");
            Console.WriteLine("2. Віднімання матриць");
            Console.WriteLine("3. Множення матриць");
            Console.WriteLine("4. Транспонування матриці");
            Console.WriteLine("5. Обернена матриця");
            Console.WriteLine("6. Визначник матриці");
            Console.WriteLine("7. Ранг матриці");
            Console.WriteLine("0. Вихід");
            Console.Write("Виберіть операцію: ");
            choice = int.Parse(Console.ReadLine());

            switch (choice)
            {
                case 0:
                    Console.WriteLine("Програма завершена.");
                    break;
                case 1:
                    Console.WriteLine("Додавання матриць:");
                    AddMatrices();
                    break;
                case 2:
                    Console.WriteLine("Віднімання матриць:");
                    SubtractMatrices();
                    break;
                case 3:
                    Console.WriteLine("Множення матриць:");
                    MultiplyMatrices();
                    break;
                case 4:
                    Console.WriteLine("Транспонування матриці:");
                    TransposeMatrix();
                    break;
                case 5:
                    Console.WriteLine("Обернена матриця:");
                    InverseMatrix();
                    break;
                case 6:
                    Console.WriteLine("Визначник матриці:");
                    CalculateDeterminant();
                    break;
                case 7:
                    Console.WriteLine("Ранг матриці:");
                    CalculateRank();
                    break;
                default:
                    Console.WriteLine("Неправильний вибір операції.");
                    break;
            }
        } while (choice != 0);
    }

    static void AddMatrices()
    {
        Console.Write("Введіть кількість рядків: ");
        int rows = int.Parse(Console.ReadLine());
        Console.Write("Введіть кількість стовпців: ");
        int columns = int.Parse(Console.ReadLine());

        Console.WriteLine("Введіть елементи першої матриці:");
        int[,] matrixA = ReadMatrix(rows, columns);

        Console.WriteLine("Введіть елементи другої матриці:");
        int[,] matrixB = ReadMatrix(rows, columns);

        int[,] resultMatrix = new int[rows, columns];

        for (int i = 0; i < rows; i++)
        {
            for (int j = 0; j < columns; j++)
            {
                resultMatrix[i, j] = matrixA[i, j] + matrixB[i, j];
            }
        }

        Console.WriteLine("Результат:");
        PrintMatrix(resultMatrix);
    }

    static void SubtractMatrices()
    {
        Console.Write("Введіть кількість рядків: ");
        int rows = int.Parse(Console.ReadLine());
        Console.Write("Введіть кількість стовпців: ");
        int columns = int.Parse(Console.ReadLine());

        Console.WriteLine("Введіть елементи першої матриці:");
        int[,] matrixA = ReadMatrix(rows, columns);

        Console.WriteLine("Введіть елементи другої матриці:");
        int[,] matrixB = ReadMatrix(rows, columns);

        int[,] resultMatrix = new int[rows, columns];

        for (int i = 0; i < rows; i++)
        {
            for (int j = 0; j < columns; j++)
            {
                resultMatrix[i, j] = matrixA[i, j] - matrixB[i, j];
            }
        }

        Console.WriteLine("Результат:");
        PrintMatrix(resultMatrix);
    }

    static void MultiplyMatrices()
    {
        Console.Write("Введіть кількість рядків першої матриці: ");
        int rowsA = int.Parse(Console.ReadLine());
        Console.Write("Введіть кількість стовпців першої матриці / кількість рядків другої матриці: ");
        int columnsA = int.Parse(Console.ReadLine());
        Console.Write("Введіть кількість стовпців другої матриці: ");
        int columnsB = int.Parse(Console.ReadLine());

        Console.WriteLine("Введіть елементи першої матриці:");
        int[,] matrixA = ReadMatrix(rowsA, columnsA);

        Console.WriteLine("Введіть елементи другої матриці:");
        int[,] matrixB = ReadMatrix(columnsA, columnsB);

        int[,] resultMatrix = new int[rowsA, columnsB];

        for (int i = 0; i < rowsA; i++)
        {
            for (int j = 0; j < columnsB; j++)
            {
                int sum = 0;
                for (int k = 0; k < columnsA; k++)
                {
                    sum += matrixA[i, k] * matrixB[k, j];
                }
                resultMatrix[i, j] = sum;
            }
        }

        Console.WriteLine("Результат:");
        PrintMatrix(resultMatrix);
    }

    static void TransposeMatrix()
    {
        Console.Write("Введіть кількість рядків: ");
        int rows = int.Parse(Console.ReadLine());
        Console.Write("Введіть кількість стовпців: ");
        int columns = int.Parse(Console.ReadLine());

        Console.WriteLine("Введіть елементи матриці:");
        int[,] matrix = ReadMatrix(rows, columns);

        int[,] resultMatrix = new int[columns, rows];

        for (int i = 0; i < rows; i++)
        {
            for (int j = 0; j < columns; j++)
            {
                resultMatrix[j, i] = matrix[i, j];
            }
        }

        Console.WriteLine("Результат:");
        PrintMatrix(resultMatrix);
    }

    static void InverseMatrix()
    {
        Console.Write("Введіть розмір квадратної матриці: ");
        int size = int.Parse(Console.ReadLine());

        Console.WriteLine("Введіть елементи матриці:");
        int[,] matrix = ReadMatrix(size, size);

        int[,] inverseMatrix = CalculateInverseMatrix(matrix);

        if (inverseMatrix == null)
        {
            Console.WriteLine("Матриця не має оберненої матриці.");
        }
        else
        {
            Console.WriteLine("Обернена матриця:");
            PrintMatrix(inverseMatrix);
        }
    }

    static int[,] CalculateInverseMatrix(int[,] matrix)
    {
        int size = matrix.GetLength(0);

        int[,] augmentedMatrix = new int[size, 2 * size];
        int[,] inverseMatrix = new int[size, size];

        for (int i = 0; i < size; i++)
        {
            for (int j = 0; j < size; j++)
            {
                augmentedMatrix[i, j] = matrix[i, j];
                augmentedMatrix[i, j + size] = (i == j) ? 1 : 0;
            }
        }

        for (int i = 0; i < size; i++)
        {
            int pivotRow = i;

            for (int j = i + 1; j < size; j++)
            {
                if (Math.Abs(augmentedMatrix[j, i]) > Math.Abs(augmentedMatrix[pivotRow, i]))
                {
                    pivotRow = j;
                }
            }

            if (augmentedMatrix[pivotRow, i] == 0)
            {
                return null; 
            }

            if (pivotRow != i)
            {
                for (int j = 0; j < 2 * size; j++)
                {
                    int temp = augmentedMatrix[i, j];
                    augmentedMatrix[i, j] = augmentedMatrix[pivotRow, j];
                    augmentedMatrix[pivotRow, j] = temp;
                }
            }

            int pivot = augmentedMatrix[i, i];

            for (int j = 0; j < 2 * size; j++)
            {
                augmentedMatrix[i, j] /= pivot;
            }

            for (int j = 0; j < size; j++)
            {
                if (j != i)
                {
                    int factor = augmentedMatrix[j, i];

                    for (int k = 0; k < 2 * size; k++)
                    {
                        augmentedMatrix[j, k] -= factor * augmentedMatrix[i, k];
                    }
                }
            }
        }

        for (int i = 0; i < size; i++)
        {
            for (int j = size; j < 2 * size; j++)
            {
                inverseMatrix[i, j - size] = augmentedMatrix[i, j];
            }
        }

        return inverseMatrix;
    }

    static void CalculateDeterminant()
    {
        Console.Write("Введіть розмір квадратної матриці: ");
        int size = int.Parse(Console.ReadLine());

        Console.WriteLine("Введіть елементи матриці:");
        int[,] matrix = ReadMatrix(size, size);

        int determinant = CalculateMatrixDeterminant(matrix);

        Console.WriteLine($"Визначник матриці: {determinant}");
    }

    static int CalculateMatrixDeterminant(int[,] matrix)
    {
        int size = matrix.GetLength(0);

        if (size == 1)
        {
            return matrix[0, 0];
        }

        int determinant = 0;

        for (int j = 0; j < size; j++)
        {
            int sign = (j % 2 == 0) ? 1 : -1;
            int subMatrixSize = size - 1;
            int[,] subMatrix = new int[subMatrixSize, subMatrixSize];

            for (int k = 1; k < size; k++)
            {
                for (int l = 0, m = 0; l < size; l++)
                {
                    if (l != j)
                    {
                        subMatrix[k - 1, m] = matrix[k, l];
                        m++;
                    }
                }
            }

            determinant += sign * matrix[0, j] * CalculateMatrixDeterminant(subMatrix);
        }

        return determinant;
    }

    static void CalculateRank()
    {
        Console.Write("Введіть кількість рядків: ");
        int rows = int.Parse(Console.ReadLine());
        Console.Write("Введіть кількість стовпців: ");
        int columns = int.Parse(Console.ReadLine());

        Console.WriteLine("Введіть елементи матриці:");
        int[,] matrix = ReadMatrix(rows, columns);

        int rank = CalculateMatrixRank(matrix);

        Console.WriteLine($"Ранг матриці: {rank}");
    }

    static int CalculateMatrixRank(int[,] matrix)
    {
        int rows = matrix.GetLength(0);
        int columns = matrix.GetLength(1);

        int rank = 0;
        int minDim = Math.Min(rows, columns);

        for (int k = 0; k < minDim; k++)
        {
            int nonZeroRow = -1;

            for (int i = k; i < rows; i++)
            {
                if (matrix[i, k] != 0)
                {
                    nonZeroRow = i;
                    break;
                }
            }

            if (nonZeroRow == -1)
            {
                continue;
            }

            if (nonZeroRow != k)
            {
                for (int j = k; j < columns; j++)
                {
                    int temp = matrix[k, j];
                    matrix[k, j] = matrix[nonZeroRow, j];
                    matrix[nonZeroRow, j] = temp;
                }
            }

            for (int i = k + 1; i < rows; i++)
            {
                int factor = matrix[i, k] / matrix[k, k];

                for (int j = k; j < columns; j++)
                {
                    matrix[i, j] -= factor * matrix[k, j];
                }
            }

            rank++;
        }

        return rank;
    }

    static int[,] ReadMatrix(int rows, int columns)
    {
        int[,] matrix = new int[rows, columns];

        for (int i = 0; i < rows; i++)
        {
            for (int j = 0; j < columns; j++)
            {
                Console.Write($"Елемент [{i}, {j}]: ");
                matrix[i, j] = int.Parse(Console.ReadLine());
            }
        }

        return matrix;
    }

    static void PrintMatrix(int[,] matrix)
    {
        int rows = matrix.GetLength(0);
        int columns = matrix.GetLength(1);

        for (int i = 0; i < rows; i++)
        {
            for (int j = 0; j < columns; j++)
            {
                Console.Write(matrix[i, j] + " ");
            }
            Console.WriteLine();
        }
    }
}
