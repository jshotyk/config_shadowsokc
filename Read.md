# Программа Калькулятор

```java
import java.util.Scanner;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Calc {
    public static void main(String[] args) {
        // Создаем объект Scanner для чтения пользовательского ввода
        Scanner scanner = new Scanner(System.in);
        // Выводим сообщение для пользователя
        System.out.println("Enter expression: ");
        // Считываем введенное пользователем выражение
        String expression = scanner.nextLine();

        // Создаем объект Pattern для работы с регулярными выражениями
        Pattern p = Pattern.compile("\".{1,10}\"\\s\\W\\s\".{1,10}\"");
        // Создаем объект Matcher для выполнения операций сопоставления с шаблоном
        Matcher m = p.matcher(expression);

        // Объявляем переменные array и action
        String[] array;
        char action;

        // Проверяем, какая арифметическая операция присутствует в выражении
        if (expression.contains("+")) {
            // Разбиваем выражение на части и сохраняем в массив array
            array = expression.split(" \\+ ");
            // Сохраняем операцию в переменной action
            action = '+';
        } else if (expression.contains("-")) {
            array = expression.split(" - ");
            action = '-';
        } else if (expression.contains("*")) {
            array = expression.split(" \\* ");
            action = '*';
        } else if (expression.contains("/")) {
            array = expression.split(" / ");
            action = '/';
        } else {
            // Если операция не найдена, генерируем исключение
            try {
                throw new Exception("This action is not found");
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        }

        // Проверяем, если операция умножения или деления, и если второе число содержит кавычки
        if (action == '*' || action == '/') {
            if (array[1].contains("\"")) {
                // Генерируем исключение, если должно быть целое число
                try {
                    throw new Exception("Should be an integer");
                } catch (Exception e) {
                    throw new RuntimeException(e);
                }
            }
        }

        // Удаляем кавычки из элементов массива array
        for (int i = 0; array.length > i; i++) {
            array[i] = array[i].replace("\"", "");
        }

        // Выполняем соответствующую операцию в зависимости от значения переменной action
        if (action == '+') {
            // Проверяем формат ввода и выводим результат
            if (!expression.matches("\".{1,10}\"\\s\\W\\s\".{1,10}\"")) {
                throw new IllegalArgumentException("Invalid input format");
            }
            printInQuotes(array[0] + array[1]);
        } else if (action == '*') {
            // Преобразуем второе число в int и выполняем умножение
            int parse = Integer.parseInt(array[1]);
            String result = "";
            // Генерируем исключение, если число не находится в диапазоне от 1 до 10
            if (parse < 1 || parse > 10) {
                try {
                    throw new Exception("Number must be between 1 and 10");
                } catch (Exception e) {
                    throw new RuntimeException(e);
                }
            }
            // Повторяем первую строку указанное количество раз
            for (int i = 0; parse > i; i++) {
                result += array[0];
            }
            // Если результат превышает 40 символов, обрезаем его
            if (result.length() > 40) {
                printInQuotes(result.substring(0, 40) + "...");
            } else {
                printInQuotes(result);
            }
        } else if (action == '-') {
            // Проверяем формат ввода и выполняем операцию вычитания
            if (!expression.matches("\".{1,10}\"\\s\\W\\s\".{1,10}\"")) {
                throw new IllegalArgumentException("Invalid input format");
            }
            // Находим индекс второй строки в первой строке и удаляем его
            int index = array[0].indexOf(array[1]);
            if (index == -1) {
                printInQuotes(array[0]);
            } else {
                String result = array[0].substring(0, index);
                result += array[0].substring(index + array[1].length());
                printInQuotes(result);
            }
        } else {
            // Делим первую строку на количество символов, указанное во второй строке
            int newLen = array[0].length() / Integer.parseInt(array[1]);
            String result = array[0].substring(0, newLen);
            printInQuotes(result);
        }

    }

    // Вспомогательный метод для вывода текста в кавычках
    static void printInQuotes(String text) {
        System.out.println("\"" + text + "\"");
    }

}
Описание программы
Данная программа представляет собой простой калькулятор, написанный на языке Java. Он выполняет базовые арифметические операции, такие как сложение, вычитание, умножение и деление. Пользователю предлагается ввести выражение, состоящее из двух строк, разделенных арифметической операцией.

Программа использует класс Scanner для чтения пользовательского ввода и классы Matcher и Pattern для работы с регулярными выражениями. Она также содержит вспомогательный метод printInQuotes, который выводит текст в кавычках.

После считывания выражения, программа анализирует его и выполняет соответствующую операцию. В зависимости от выбранной операции, программа может генерировать исключения, если ввод не соответствует ожидаемому формату или если указанные числа находятся в неправильном диапазоне.

В конце выполнения программы выводится результат операции в кавычках
