import java.util.*;

public class Main {
    private static int index;
    public static void main(String[] args) {////метод main является точкой входа в программу – с него начинается исполнение
        System.out.println("Ввод выражения (Для выхода введите quit): ");
        Scanner in = new Scanner(System.in);
        while (in.hasNextLine()) {

            String primer = in.nextLine();////этот метод читает и воспроизводит данные до конца строки до тех пор, пока не начнется новая строка или старая не будет разорвана с помощью \n или Enter
            if (primer.contains("/") && primer.substring(primer.indexOf("/") + 1).startsWith("0")) { ////substring() возвращает новую строку, которая является подстрокой данной строки; indexOf() ищет в строке заданный символ или строку, и их возвращает индекс (т.е. порядковый номер); startsWith() возвращает значение true, если последовательность символов, представленного аргумента является префиксом последовательности символов, представляемой данной строкой
                System.out.println("Ошибка. Деление на ноль");
            }
            if (primer.matches(".*([+\\-*/])\\1+.*")) {
                System.out.println("Ошибка. Некорректное выражение");
                break;
            }

            if (primer.equals("quit")) { ////equals() проверяет, равны ли два объекта (например, две строки)
                System.out.println("Завершение работы...");
                break;
            }

            index = 0;
            System.out.println(prepare(primer));
        }
    }
    public static String prepare(String currentprimer) {
        Stack <String> chisla = new Stack<>(); //// Наш Stack элементов, Stack является подклассом класса Vector, который реализует простой механизм типа "последний вошёл - первый вышел" 

        char operator = ' ';

        while (index < currentprimer.length()) { //// Проходимся по всем элементам выражения
            char tut = currentprimer.charAt(index); //// Кидаем наш элементу, harAt() – возвращает символ, расположенный по указанному индексу строки
            if (tut == ' ') { // Если пробел то переходим к следующему элементу
                index++; ////Увеличивает каждый элемент объекта 
                continue;
            }
            if (tut == ')') {
                index++;
                break;
            } else if (tut == '(') {
                index++;
                String otvet = prepare(currentprimer); /// otvet = ответ в cкобках
                if (otvet.equals("null")) {
                    System.out.println("Недопустимый ввод скобки пустые"); // Тест на недопустимые значения
                    return "null";
                }

                if (operator == ' ' || operator == '+') {
                    chisla.push(otvet);
                } else if (operator == '-') {
                    chisla.push(calc("-1/1", '*', otvet)); // Минус представляем как дробь
                } else if (operator == '*') {
                    chisla.push(calc(chisla.pop(), operator, otvet));
                } else if (operator == ':') {
                    chisla.push(calc(chisla.pop(), operator, otvet));
                } else {
                    System.out.println("Недопустимый символ"); // Тест на недопустимый символ
                    return "null";
                }
            } else if (Character.isDigit(tut)) { ////Character.isDigit() определяет, является ли указанное значение типа char цифрой

                int chislitel_a = 0;
                int znamenatel_a = 0;
                int otricatelnoe_up = 1;   //// определяет отрицательное число или нет
                int otricatelnoe_down = 1;   //// определяет отрицательное число или нет
                boolean chislitel = true;
                char tuta = currentprimer.charAt(index);////chartAt (int index) возвращает символ, стоящий на определенном индексе

                if (index > 0 && currentprimer.charAt(index - 1) == '-') { //// отрицательный числитель
                    otricatelnoe_up = -1;
                }
                while ((Character.isDigit(tuta) || tuta == '-' || tuta == '/') && index < currentprimer.length()) {
                    if (tuta == '-') {
                        if (index > 0 && currentprimer.charAt(index - 1) != '/') { ///// 1/11-1  1-1/1
                            System.out.println("Ошибка: отрицательная дробь неправильная");
                        } else {
                            otricatelnoe_down = -1;
                        }
                        index++;
                        tuta = currentprimer.charAt(index);
                        continue;
                    }

                    if (tuta == '/') {
                        chislitel = false;
                        index++;
                        tuta = currentprimer.charAt(index);
                        continue;
                    }

                    if (chislitel) {
                        chislitel_a = chislitel_a * 10 + otricatelnoe_up * (tuta - '0');
                    } else {
                        znamenatel_a = znamenatel_a * 10 + otricatelnoe_down * (tuta - '0');
                    }
                    index++;
                    if (index < currentprimer.length()) tuta = currentprimer.charAt(index);
                }

                if (znamenatel_a < 0) {
                    chislitel_a *= -1;
                    znamenatel_a *= -1;
                }

                if (chislitel_a < 0 && znamenatel_a < 0) {
                    chislitel_a *= -1;
                    znamenatel_a *= -1;
                }

                String chislo = Integer.toString(chislitel_a) + "/" + Integer.toString(znamenatel_a);////toString() возвращает строковый объект, представляющий заданное целое число

                if (operator == '-') {
                    chislo = calc("-1/1", '*', chislo);
                } else if (operator == '*') {
                    chislo = calc(chisla.pop(), operator, chislo);
                } else if (operator == ':') {
                    chislo = calc(chisla.pop(), operator, chislo);
                }
                chisla.push(chislo);
            } else {
                if (tut == '*') {
                    operator = tut;
                } else if (tut == ':') {
                    operator = tut;
                } else if (tut == '-' && index + 1 < currentprimer.length() && !Character.isDigit(currentprimer.charAt(index + 1))) {
                    operator = tut;
                } else if (tut == '+') {
                    operator = tut;
                } else if (tut != '-' && index + 1 < currentprimer.length() && Character.isDigit(currentprimer.charAt(index + 1))){
                    System.out.println("Недопустимый ввод");
                }
                index++;
            }
        }

        if (chisla.size() == 0) { // Пусто
            return "null";
        }

        String result = chisla.pop();
        while (chisla.size() > 0) {
            String last = chisla.pop();
            result = calc(result, '+', last);
        }
        return result;
    }
    public static String calc(String a, char operator, String b) {
        int chislitel_a = 0;
        int znamenatel_a = 0;
        int otricatelnoe_up = 1;   //// определяет отрицательное число или нет
        int otricatelnoe_down = 1;   //// определяет отрицательное число или нет
        boolean chislitel = true;
        for (int i = 0; i < a.length(); i++) {
            char tut = a.charAt(i);
            if (tut == '-') {
                if (i > 0 && a.charAt(i - 1) != '/') {
                    System.out.println("Ошибка: отрицательная дробь неправильная");
                } else {
                    if (chislitel) {
                        otricatelnoe_up = -1;
                    } else {
                        otricatelnoe_down = -1;
                    }
                }
                continue;
            }

            if (tut == '/') {
                chislitel = false;
                continue;
            }

            if (chislitel) {
                chislitel_a = chislitel_a * 10 + otricatelnoe_up * (tut - '0');
            } else {
                znamenatel_a = znamenatel_a * 10 + otricatelnoe_down * (tut - '0');
            }
        }

        if (znamenatel_a < 0) {
            chislitel_a *= -1;
            znamenatel_a *= -1;
        }

        if (chislitel) {
            System.out.println("Ошибка нет знака \'/\'");
            return "null";
        }
        //та же самая обработка со второй дробью
        int chislitel_b = 0;
        int znamenatel_b = 0;
        otricatelnoe_up = 1;
        otricatelnoe_down = 1;
        chislitel = true;
        for (int i = 0; i < b.length(); i++) {
            char tut = b.charAt(i);
            if (tut == '-') {
                if (i > 0 && b.charAt(i - 1) != '/') {
                    System.out.println("Ошибка: отрицательная дробь неправильная");
                } else {
                    if (chislitel) {
                        otricatelnoe_up = -1;
                    } else {
                        otricatelnoe_down = -1;
                    }
                }
                continue;
            }
            if (tut == '/') {
                chislitel = false;
                continue;
            }

            if (chislitel) {
                chislitel_b = chislitel_b * 10 + otricatelnoe_up * (tut - '0');
            } else {
                znamenatel_b = znamenatel_b * 10 + otricatelnoe_down * (tut - '0');
            }
        }

        if (znamenatel_b < 0) {
            chislitel_b *= -1;
            znamenatel_b *= -1;
        }

        if (chislitel) {
            System.out.println("Ошибка: нет знака \'/\'");
            return "null";
        }

        int result_chislitel = chislitel_a;
        int result_znamenatel = znamenatel_a;
        if (operator == '+') {
            result_chislitel = chislitel_a * znamenatel_b + chislitel_b * znamenatel_a;
            result_znamenatel = znamenatel_a * znamenatel_b;
        }
        if (operator == '-') {
            result_chislitel = chislitel_a * znamenatel_b - chislitel_b * znamenatel_a;
            result_znamenatel = znamenatel_a * znamenatel_b;
        }
        if (operator == '*') {
            result_chislitel *= chislitel_b;
            result_znamenatel *= znamenatel_b;
        }
        if (operator == ':') {
            result_chislitel *= znamenatel_b;
            result_znamenatel *= chislitel_b;
        }
        // Упрощаем дроби
        int naibolshii_obshii_delitel = gcd(Math.abs(result_chislitel), Math.abs(result_znamenatel));
        result_chislitel /= naibolshii_obshii_delitel;
        result_znamenatel /= naibolshii_obshii_delitel;
        if (result_znamenatel < 0) {
            result_chislitel *= -1;
            result_znamenatel *= -1;
        }

        if (result_chislitel < 0 && result_znamenatel < 0) {
            result_chislitel *= -1;
            result_znamenatel *= -1;
        }
        String result = Integer.toString(result_chislitel) + "/" + Integer.toString(result_znamenatel);
        return result;
    }
    public static int gcd(int a, int b) {
        if (b == 0) return a;
        else return gcd(b, a % b);
    }
}
