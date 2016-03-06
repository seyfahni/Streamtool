# Streamtool

package main;

import java.awt.Container;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.HashMap;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;

public class Stream extends JFrame {
	
	public static void main(String[] args) {
		new Stream();
	}

	private static final long serialVersionUID = 1L;
	private JButton increase = new JButton("Kills +");
	private JButton decrease = new JButton("Death +");
	private JButton increase1 = new JButton("Kills -");
	private JButton decrease1 = new JButton("Death -");
	public static JLabel show = new JLabel("");
	public static HashMap<Double, Double> kd = new HashMap<Double, Double>();
	public static double kills = 1;
	public static double death = 1;
	public static double kdr = 0;
	
	public Stream() {
		setDefaultCloseOperation(1);
	    setSize(200,150);
	    setResizable(false);
	    Container cp = getContentPane();
	    cp.setLayout(new GridLayout(0,2));
	    
	    cp.add(increase);
	    cp.add(decrease);
	    cp.add(increase1);
	    cp.add(decrease1);
	    cp.add(show);
	    
	    increase.addActionListener(new ActionListener() { 
	        public void actionPerformed(ActionEvent evt) { 
	        	kills++;
	        	doIT();
	          }
	        });
	    
	    increase1.addActionListener(new ActionListener() { 
	        public void actionPerformed(ActionEvent evt) { 
	        	kills--;
	        	doIT();
	          }
	        });
	    
	    decrease.addActionListener(new ActionListener() { 
	        public void actionPerformed(ActionEvent evt) { 
	        	death++;
	        	doIT();
	          }
	        });
	    
	    decrease1.addActionListener(new ActionListener() { 
	        public void actionPerformed(ActionEvent evt) { 
	        	death--;
	        	doIT();
	          }
	        });
	    try {
	    	load();
	    } catch(Exception e) {
	    	System.out.println("Coudnt load! File existing?");
	    }
	    
		setVisible(true);
	}
	
	public void load() throws IOException {
		@SuppressWarnings("resource")
		BufferedReader ois1 = new BufferedReader(new FileReader("KD.txt"));
		String line = ois1.readLine();
		show.setText(line);
		try {
			kills = Integer.parseInt(String.valueOf(line.charAt(5)));
			death = Integer.parseInt(String.valueOf(line.charAt(9)));  
        } catch(Exception e) {
        	System.out.println("File empty :) First use?");
        }

	}
	
	public void doIT() {
		kd.put(kills, death);
    	show.setText(Calculate(kills,death));
	}
	
	public static String Calculate(Double kills, Double death) {
		double kdr = kills / death;
		if(String.valueOf(kdr).equals("Infinity") || String.valueOf(kdr).equals("NaN")) {
			kdr = 666;
		}
		String thefinal = null;
		try (BufferedWriter bw = new BufferedWriter(new FileWriter("KD.txt", false))) {
			String k = String.valueOf(Math.round(kills));
			String d = String.valueOf(Math.round(death));
			String r = String.valueOf(Math.round(kdr*100.0)/100.0);
			thefinal = "KDR: "+k+" / "+d+" | "+r;
			bw.write(thefinal);
			bw.newLine();
			bw.flush();
		}
		catch (IOException ex) {
			System.out.println("Error!");
		}
		return thefinal;
	}
}
