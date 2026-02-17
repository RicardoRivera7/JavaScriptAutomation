# JavaScriptAutomation


import java.util.*;
import java.io.*;
public class Yu_Gi_Oh 
{

	
	public static void main(String[] args) 
	{
		String opp = null;
		
		
		
		System.out.println("Welcome to Yu-Gi-Oh the game!\n");
		
	
		
		
		
			System.out.println("This is a simplified version of Yu-Gi-Oh, you simply choose a card and that card will automatically attack.");
			System.out.println("The cards do damage to both players so choose your cards wisely!");
			System.out.println("once you place a card it is gone after the battle, and once you draw all of the cards in your hand shift position down");
			System.out.println("Have fun\n");
			
		
		String character = PickACharacter();
		CardsAndDecks player1 = new CardsAndDecks();
		PlayerStats player = new PlayerStats(4000, 7);
		AI com = new AI(4000, 7);
		String choosedeck = player1.ChooseDeck();
		player.PlayerHealth1(character);
		opp = com.AIselect(opp);
		com.AIhealth1(opp);
	
		player1.DuelMonsterscards(choosedeck);
		System.out.println("\nIt's time to D-d-d-duel!!!");
		boolean run = true;
		
		while(run)
		{
			player1.battle();
			
			com.draw();
			
				player.PlayerHealth(character,com.getAttack(opp));
				com.AIhealth(opp, player1.getAttack(character));
				
				player1.draw(choosedeck);
		
		if(player.isDead()==true || com.AIisDead()==true)
		{
			run = false;
		
		if(player.isDead()==true)
		{
			System.out.println("Game over you lose, you have been banished to the shadow realm");
		}
		if(com.AIisDead()==true)
		{
			System.out.println("Congrats!!! you won the duel!");
		}
		
		}
		}
	}
	
	

	
	public static String PickACharacter()
	{
		@SuppressWarnings("resource")
		Scanner in = new Scanner(System.in);
		
		System.out.println("Please select your character:\n");
		System.out.println("Yami Yugi");
		System.out.println("Seto Kaiba");
		System.out.println("Joey Wheeler");
		System.out.println("Mai Valentine");
		System.out.println("Tea Gardner");
		
		String character = in.nextLine();
		
		switch (character.charAt(0))
		{
		
			case 'Y':
			case 'y':
			{
				System.out.println("You have selected Yami Yugi");
				String charselect = "Yami Yugi";
				return charselect;
				
			}
			
			case 'S':
			case 's':
			{
				System.out.println("you have selected Seto Kaiba");
				String charselect = "Seto Kaiba";
				return charselect;
				
			}
			
			case 'J':
			case 'j':
			{
				System.out.println("you have selected Joey Wheeler");
				String charselect = "Joey Wheeler";
				return charselect;
			}
			
			case 'M':
			case 'm':
			{
				System.out.println("You have selected Mai Valentine");
				String charselect = "Mai Valentine";
				return charselect;
				
			}
			
			case 'T':
			case 't':
			{
				System.out.println("You have selected Tea Gardner");
				String charselect = "Tea Gardner";
				return charselect;
			}
			default:
			{
				System.out.println("you have not selected any of the characters, you will then be player 1");
				String charselect = "Player 1";
				return charselect;
			}
		
		}
		
	}

}
