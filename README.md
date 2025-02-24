git init
git add -A
git commit -m 'Added my project'
git remote add origin git@github.com:nikabelova08/newproject23.git
git push -u  origin main
import java.util.Scanner;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

class Main {


    public static String calc(String input) throws Exception {
       
        input = input.replaceAll("\\s+", "");

       
        Pattern pattern = Pattern.compile("^(I{1,3}|IV|V|VI|VII|VIII|IX|X)[\\+\\-\\*/](I{1,3}|IV|V|VI|VII|VIII|IX|X)$|^(10|[1-9])[\\+\\-\\*/](10|[1-9])$");
        Matcher matcher = pattern.matcher(input);

        if (!matcher.matches()) {
            throw new Exception("Неверный ввод.");
        }

        String[] parts;
        int num1, num2;
        String operator;

        
        if (input.matches("^(I{1,3}|IV|V|VI|VII|VIII|IX|X)[\\+\\-\\*/](I{1,3}|IV|V|VI|VII|VIII|IX|X)$")) {
            parts = input.split("[\\+\\-\\*/]");
            num1 = romanToArabic(parts[0]);
            num2 = romanToArabic(parts[1]);
            operator = String.valueOf(input.charAt(parts[0].length())); 
        } else {
            parts = input.split("[\\+\\-\\*/]");
            num1 = Integer.parseInt(parts[0]);
            num2 = Integer.parseInt(parts[1]);
            operator = String.valueOf(input.charAt(parts[0].length())); 
        }

       
        int result;
        switch (operator) {
            case "+":
                result = num1 + num2;
                break;
            case "-":
                result = num1 - num2;
                break;
            case "*":
                result = num1 * num2;
                break;
            case "/":
                if (num2 == 0) {
                    throw new Exception("Деление на ноль.");
                }
                result = num1 / num2; 
                break;
            default:
                throw new Exception("Неверный оператор.");
        }

       
        if (input.matches("^(I{1,3}|IV|V|VI|VII|VIII|IX|X)[\\+\\-\\*/](I{1,3}|IV|V|VI|VII|VIII|IX|X)$") && result < 1) {
            throw new Exception("Результат должен быть больше нуля для римских чисел.");
        }

      
        return input.matches("^(I{1,3}|IV|V|VI|VII|VIII|IX|X)[\\+\\-\\*/](I{1,3}|IV|V|VI|VII|VIII|IX|X)$") ? arabicToRoman(result) : String.valueOf(result);
    }

    private static int romanToArabic(String roman) {
        switch (roman) {
            case "I": return 1;
            case "II": return 2;
            case "III": return 3;
            case "IV": return 4;
            case "V": return 5;
            case "VI": return 6;
            case "VII": return 7;
            case "VIII": return 8;
            case "IX": return 9;
            case "X": return 10;
            default: throw new IllegalArgumentException("Неверное римское число.");
        }
    }

    private static String arabicToRoman(int number) {
        if (number <= 0) {
            throw new IllegalArgumentException("Результат должен быть положительным для римских чисел.");
        }

        StringBuilder roman = new StringBuilder();
        while (number >= 10) {
            roman.append("X");
            number -= 10;
        }
        while (number >= 9) {
            roman.append("IX");
            number -= 9;
        }
        while (number >= 5) {
            roman.append("V");
            number -= 5;
        }
        while (number >= 4) {
            roman.append("IV");
            number -= 4;
        }
        while (number >= 1) {
            roman.append("I");
            number -= 1;
        }
        return roman.toString();
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Введите выражение (например, I + II или 3 * 4):");

        while (true) {
            String input = scanner.nextLine();
            if (input.equalsIgnoreCase("exit")) {
                break;
            }

            try {
                String result = calc(input);
                System.out.println("Результат: " + result);
            } catch (Exception e) {
                System.out.println(e.getMessage());
            }
        }

        scanner.close();
    }
}
