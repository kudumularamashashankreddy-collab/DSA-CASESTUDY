import java.util.Scanner;

public class GenomicSequencePipeline {

    // DNA Sequence
    static String dna;

    // Prefix count array
    static int[][] prefix;

    // Function to convert nucleotide to index
    static int getIndex(char ch) {
        switch (ch) {
            case 'A':
                return 0;
            case 'C':
                return 1;
            case 'G':
                return 2;
            case 'T':
                return 3;
            default:
                return -1;
        }
    }

    // Preprocessing function
    static void preprocess() {

        int n = dna.length();

        prefix = new int[4][n + 1];

        for (int i = 0; i < n; i++) {

            // Copy previous counts
            for (int j = 0; j < 4; j++) {
                prefix[j][i + 1] = prefix[j][i];
            }

            // Increase count of current nucleotide
            int idx = getIndex(dna.charAt(i));
            prefix[idx][i + 1]++;
        }
    }

    // Range-Rank Query Function
    static int rangeRank(char nucleotide, int L, int R) {

        int idx = getIndex(nucleotide);

        if (idx == -1) {
            return -1;
        }

        return prefix[idx][R] - prefix[idx][L - 1];
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // Input DNA sequence
        System.out.println("Enter DNA Sequence (Only A, C, G, T): ");
        dna = sc.nextLine().toUpperCase();

        // Preprocess sequence
        preprocess();

        // Input query details
        System.out.println("Enter nucleotide to count: ");
        char nucleotide = sc.next().toUpperCase().charAt(0);

        System.out.println("Enter starting position (L): ");
        int L = sc.nextInt();

        System.out.println("Enter ending position (R): ");
        int R = sc.nextInt();

        // Validate range
        if (L < 1 || R > dna.length() || L > R) {
            System.out.println("Invalid Range!");
        } else {

            int result = rangeRank(nucleotide, L, R);

            if (result == -1) {
                System.out.println("Invalid Nucleotide!");
            } else {

                // Display output
                System.out.println("\nDNA Sequence: " + dna);

                System.out.println(
                    "Occurrences of " + nucleotide +
                    " from position " + L +
                    " to " + R +
                    " = " + result
                );
            }
        }

        sc.close();
    }
}
