import java.io.File;
import java.io.FileNotFoundException;
import java.util.ArrayList;
import java.util.List;
import java.util.*;

public class dfaTranslator {

    public static String sortString(String inputString)
    {
        //index to track the location of next character(Input string)
        int index1 = 1;

        // index to track the location of next character(Resultant string)
        int index2 = 1;

        // Converting input string to character array
        char tempArray[] = inputString.toCharArray();

        // Sorting temp array using
        Arrays.sort(tempArray);

        while (index1 != tempArray.length){
            if (tempArray[index1] != tempArray[index1-1]){
                tempArray[index2] = tempArray[index1];
                index2++;
            }
            index1++;
        }

        // Returning new sorted string
        String tempString = new String(tempArray);
        return tempString.substring(0, index2);
    }

    public static void main(String[] args) throws FileNotFoundException {

        //Creates the ArrayLists to store the values
        List<String> alphabet = new ArrayList<>();
        List<String> states = new ArrayList<>();
        List<String> start = new ArrayList<>();
        List<String> accept = new ArrayList<>();
        List<String> transition = new ArrayList<>();

        //Stores the location of the file
        File fis = new File("./src/NFA2.txt");

        //Creates a scanner to scan in the file
        Scanner sc = new Scanner(fis);

        //Keeps track of what point in the file we are
        int tracker = 0;

        //String to store contents to be used
        String temp;

        //Loops through until no lines are left to read
        while(sc.hasNext())
        {
            temp = sc.nextLine();
            //Checks what part of the information is being read in
            switch (temp){
                case "ALPHABET":
                    temp = sc.nextLine();
                    break;
                case "STATES":
                    tracker = 1;
                    temp = sc.nextLine();
                    break;
                case "START":
                    tracker = 2;
                    temp = sc.nextLine();
                    break;
                case "FINAL":
                    tracker = 3;
                    temp = sc.nextLine();
                    break;
                case "TRANSITIONS":
                    tracker = 4;
                    temp = sc.nextLine();
                    break;
                default:
                    break;
            }
            //Adds the information to the appropriate ArrayList
            switch (tracker){
                case 0:
                    alphabet.add(temp);
                    break;
                case 1:
                    states.add(temp);
                    break;
                case 2:
                    start.add(temp);
                    break;
                case 3:
                    accept.add(temp);
                    break;
                case 4:
                    transition.add(temp);
                    break;
            }
        }
        //Removes END from the transition array
        transition.remove(transition.size()-1);
        //Closes the scanner
        sc.close();


        //Creates the array I will be working with, with as many arrays as there as states
        ArrayList<ArrayList<String>> myArray = new ArrayList<>(states.size());

        //Initializes the array with "-5" default value, as many as there are alphabet characters
        for(int i=0; i < states.size(); i++) {
            myArray.add(new ArrayList());
            for (int j = 0; j < alphabet.size(); j++){
                myArray.get(i).add("-5");
            }
        }

        //Reads the transitions and places the appropriate information into the array
        for (int i = 0; i<transition.size(); i++){
            char source = transition.get(i).charAt(0);
            int index = states.indexOf(String.valueOf(source));
            source = transition.get(i).charAt(2);
            int index1 = alphabet.indexOf(String.valueOf(source));
            if (myArray.get(index).get(index1) != "-5" && !myArray.get(index).get(index1).contains(String.valueOf(transition.get(i).charAt(4)))){
                myArray.get(index).set(index1, myArray.get(index).get(index1).concat(String.valueOf(transition.get(i).charAt(4))));

            }
            else if (!myArray.get(index).get(index1).contains(String.valueOf(transition.get(i).charAt(4)))){
                myArray.get(index).set(index1, String.valueOf(transition.get(i).charAt(4)) );
            }
        }

        int checker = 0;
        while (checker == 0) {

            //Count to keep track of how many new states there are
            int newCount = 0;
            //Adds a new state to the ArrayList for each new state encountered
            for (int i = 0; i < myArray.size(); i++){
                for (int j = 0; j < myArray.get(i).size(); j++){
                    if(myArray.get(i).get(j) != "-5"){
                        String outputString = sortString(myArray.get(i).get(j));
                        myArray.get(i).set(j, outputString);
                    }
                    if (!states.contains(myArray.get(i).get(j)) && myArray.get(i).get(j) != "-5"){
                        states.add(myArray.get(i).get(j));
                        newCount++;
                    }
                }
            }

            if (newCount == 0){
                checker = 1;
                break;
            }

            //Loops through each new state and creates a new ArrayList in myArray, finds and adds the appropriate information
            for (int i = states.size()-newCount; i < states.size(); i++){
                myArray.add(new ArrayList());
                for (int j = 0; j < alphabet.size(); j++){
                    myArray.get(i).add("-5");
                    for (int f = 0; f < states.get(i).length(); f++){
                        char source = states.get(i).charAt(f);
                        int index = states.indexOf(String.valueOf(source));
                        String temp1 = myArray.get(index).get(j);
                        if (myArray.get(i).get(j) != "-5" && temp1 != "-5" && !myArray.get(i).get(j).contains(temp1)){
                            myArray.get(i).set(j, myArray.get(i).get(j).concat(temp1));
                        }
                        else if (temp1 != "-5" && !myArray.get(i).get(j).contains(temp1)){
                            myArray.get(i).set(j, temp1);
                        }
                    }
                }
            }

        }


        for (int i = 0; i < states.size(); i++){
            for (int f = 0; f < accept.size(); f++) {
                if (states.get(i).contains(accept.get(f)) && !accept.contains(states.get(i))){
                    accept.add(states.get(i));
                }
            }
        }


        System.out.println("ALPHABET");
        for (int i = 0; i < alphabet.size(); i++){
            System.out.println(alphabet.get(i));
        }

        System.out.println("STATES");
        for (int i = 0; i < states.size(); i++){
            System.out.println(states.get(i));
        }

        System.out.println("START");
        for (int i = 0; i < start.size(); i++){
            System.out.println(start.get(i));
        }

        System.out.println("FINAL");
        for (int i = 0; i < accept.size(); i++){
            System.out.println(accept.get(i));
        }

        System.out.println("TRANSITIONS");

        for (int i = 0; i < states.size(); i++){
            for (int j = 0; j < alphabet.size(); j++){
                if (myArray.get(i).get(j) != "-5"){
                    System.out.print(states.get(i));
                    System.out.print(" ");
                    System.out.print(alphabet.get(j));
                    System.out.print(" ");
                    System.out.print(myArray.get(i).get(j));
                    System.out.println("");
                }
            }
        }
        System.out.println("END");

    }

}
