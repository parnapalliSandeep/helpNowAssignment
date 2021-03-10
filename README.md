#helpNowAssignment
import java.util.Map;
import java.util.HashMap;
import java.text.DateFormat;
import java.util.Date;
import java.text.SimpleDateFormat;
import java.util.Scanner;

class FunFactory {
    private Scanner scanner=new Scanner(System.in);
    private Map<String,Integer> usersList=new HashMap<>();

    public void regUser(){
        boolean flag=true;
        while(flag){
            System.out.print("please enter the your name:");
            String name=scanner.nextLine();
            if(usersList.containsKey(name)){
                System.out.println("sorry, same username is already present,please try a different one");
            }else{
                System.out.println("username is available,we are creating an user with your name and adding an" +
                        " initial balance of 50 rupees ");
                usersList.put(name,50);
                flag=false;
            }
        }
    }

    public void rechargeCounter(String name){
        System.out.println("enter the amount you would like to recharge");
        int amount=scanner.nextInt();
        scanner.nextLine();
        int updatedBalance=amount+usersList.get(name);
        usersList.put(name,updatedBalance);
        System.out.println("recharge successful,your updated balance is "+updatedBalance);
    }

    public int viewBalance(){
        int option;
        int balance=0;
        System.out.print("please swipe your card to view the balance:");
        String name=scanner.nextLine();
        if(usersList.containsKey(name)){
            balance=usersList.get(name);
        }else{
            System.out.println("this is not a valid card, please contact customer care to get a new card");
            System.out.println("press 1 if you would like to buy a new card,else press 2");
            option=scanner.nextInt();
            scanner.nextLine();
            if(option==1){
                regUser();
            }
        }
        return balance;
    }

    public void listUsers(){
        System.out.println(usersList.toString());
    }


    public int ticketPrice(){
        int ticketPrice=0;
        Date date = new Date();
        DateFormat dateFormat = new SimpleDateFormat("EEE");
        String currentDate = dateFormat.format(date);
        if(currentDate.equals("Sat") || currentDate.equals("Sun")) {
            ticketPrice=20;
        }else{
            ticketPrice=10;
        }
        return ticketPrice;
    }

    public void subMethod(int i,int ticketPrice){
        int option;
        System.out.println("welcome user, please swipe your card if you would like to play game" +
                +i+" else move to next game");//in our case swiping means enter the user name
        String name=scanner.nextLine();//press enter to move to next game
        if(name.isEmpty()){
            System.out.println("moving on to next game");
        }else {
            if (usersList.containsKey(name)) {
                if (usersList.get(name) >= 10) {
                    System.out.println("Entry allowed, you have required minimum balance");
                    int updatedBal=usersList.get(name)-ticketPrice;
                    usersList.put(name,updatedBal);
                } else {
                    System.out.println("you dont have required balance," +
                            "press 1 to move to customercare to recharge your card");
                    option = scanner.nextInt();
                    scanner.nextLine();
                    if (option == 1) {
                        rechargeCounter(name);
                    }
                }
            } else {
                System.out.println("this is not a valid card, please contact customer care to get a new card");
                System.out.println("press 1 if you would like to buy a new card,else press 2");
                option=scanner.nextInt();
                scanner.nextLine();
                if(option==1){
                    regUser();
                }
            }
        }
    }
    public void gamingCardSystem(){
        int option;
        System.out.println("welcome to funfactory, we have 10 games available for you today");
        int ticketPrice=ticketPrice();
        System.out.println("ticket price of each game is "+ ticketPrice+" ,please make sure you have sufficient" +
                " balance in your account");
        System.out.print("press 1 if you would like to buy a new card, else press 2:");
        option=scanner.nextInt();
        scanner.nextLine();
        if(option==1){
            regUser();
        }
        int balance=viewBalance();
        System.out.println("the balance in your account is "+balance);
        System.out.println("you can start playing from game 1 to 10 or vice versa");
        System.out.print("press 1 if you are starting your game at game 1, else press 2:");
        option=scanner.nextInt();
        scanner.nextLine();
        if(option==1){
            for(int i=1;i<=10;i++){
                subMethod(i,ticketPrice);
            }
        }else{
            for(int i=10;i>=0;i--){
                subMethod(i,ticketPrice);
            }
        }

        }
    }


public class Main {
    public static void main(String[] args) {
        FunFactory funFactory=new FunFactory();
        funFactory.gamingCardSystem();
    }
}
