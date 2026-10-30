SOURCE CODE

import java.util.ArrayDeque;

import java.util.Deque;

import java.util.Scanner;

public class DelimiterChecker {

    // Returns true when ch is one of ( [ {
    static boolean isOpening(char ch) {
        return ch == '(' || ch == '[' || ch == '{';
    }

    // Returns true when ch is one of ) ] }
    static boolean isClosing(char ch) {
        return ch == ')' || ch == ']' || ch == '}';
    }

    // Returns true when open is the correct partner of close
    static boolean matches(char open, char close) {
        return (open == '(' && close == ')')
            || (open == '[' && close == ']')
            || (open == '{' && close == '}');
    }

    // Checks the sequence from left to right using a stack.
    // Returns "Valid" or "Invalid - <reason>".
    static String check(String input) {
        Deque<Character> stack = new ArrayDeque<>();

        for (int i = 0; i < input.length(); i++) {
            char ch = input.charAt(i);

            if (isOpening(ch)) {
                stack.push(ch);                              // PUSH every opening delimiter
            } else if (isClosing(ch)) {
                if (stack.isEmpty()) {                       // nothing to match with
                    return "Invalid - closing delimiter '" + ch + "' at position "
                         + (i + 1) + " has no matching opening delimiter";
                }
                char top = stack.peek();                     // PEEK at the most recent opener
                if (!matches(top, ch)) {
                    return "Invalid - mismatched delimiters: '" + ch + "' at position "
                         + (i + 1) + " does not match '" + top + "' on top of the stack";
                }
                stack.pop();                                 // POP the matched opener
            }
            // any other character is ignored
        }

        if (!stack.isEmpty()) {
            return "Invalid - unmatched opening delimiter remains: '" + stack.peek() + "'";
        }
        return "Valid";
    }

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        System.out.println("=== Matching Delimiters Checker (Stack) ===");
        System.out.println("Type 'exit' to quit. Press Enter on a blank line to test empty input.");

        while (true) {
            System.out.print("\nEnter a sequence: ");
            if (!in.hasNextLine()) break;
            String input = in.nextLine();
            if (input.trim().equalsIgnoreCase("exit")) break;

            System.out.println("Input : " + (input.isEmpty() ? "(empty)" : input));
            System.out.println("Result: " + check(input));
        }
        in.close();
        System.out.println("Program ended.");
    }
}


MODIFIED CODE

import java.util.ArrayDeque;

import java.util.Deque;

import java.util.Iterator;

import java.util.Scanner;

public class DelimiterCheckerTrace {

    static boolean isOpening(char ch) {
        return ch == '(' || ch == '[' || ch == '{';
    }

    static boolean isClosing(char ch) {
        return ch == ')' || ch == ']' || ch == '}';
    }

    static boolean matches(char open, char close) {
        return (open == '(' && close == ')')
            || (open == '[' && close == ']')
            || (open == '{' && close == '}');
    }

    // Formats the stack from bottom to top, e.g. "{ [ (" or "(empty)"
    static String show(Deque<Character> stack) {
        if (stack.isEmpty()) return "(empty)";
        StringBuilder sb = new StringBuilder();
        Iterator<Character> it = stack.descendingIterator();   // bottom -> top
        while (it.hasNext()) {
            sb.append(it.next());
            if (it.hasNext()) sb.append(' ');
        }
        return sb.toString();
    }

    static boolean check(String input) {
        Deque<Character> stack = new ArrayDeque<>();
        System.out.println("Input: " + (input.isEmpty() ? "(empty)" : input));

        for (int i = 0; i < input.length(); i++) {
            char ch = input.charAt(i);

            if (isOpening(ch)) {
                System.out.println("Read: " + ch);
                stack.push(ch);
                System.out.println("Operation: PUSH " + ch);
                System.out.println("Stack: " + show(stack));
            } else if (isClosing(ch)) {
                System.out.println("Read: " + ch);
                if (stack.isEmpty()) {
                    System.out.println("Operation: PEEK -> stack is empty");
                    System.out.println("Reason: closing delimiter '" + ch
                        + "' has no matching opening delimiter");
                    return false;
                }
                char top = stack.peek();
                System.out.println("Operation: PEEK -> " + top);
                if (!matches(top, ch)) {
                    System.out.println("Reason: mismatched delimiters - '" + ch
                        + "' does not match '" + top + "' on top of the stack");
                    System.out.println("Stack: " + show(stack));
                    return false;
                }
                stack.pop();
                System.out.println("Operation: POP " + top);
                System.out.println("Stack: " + show(stack));
            }
            // other characters are ignored
        }

        if (!stack.isEmpty()) {
            System.out.println("Reason: unmatched opening delimiter remains - stack: "
                + show(stack));
            return false;
        }
        System.out.println("End of input: stack is empty");
        return true;
    }

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        System.out.println("=== Matching Delimiters Checker with Stack Trace ===");
        System.out.println("Type 'exit' to quit. Press Enter on a blank line to test empty input.");

        while (true) {
            System.out.print("\nEnter a sequence: ");
            if (!in.hasNextLine()) break;
            String input = in.nextLine();
            if (input.trim().equalsIgnoreCase("exit")) break;

            System.out.println();
            boolean ok = check(input);
            System.out.println("Result: " + (ok ? "Valid" : "Invalid"));
        }
        in.close();
        System.out.println("Program ended.");
    }
}
