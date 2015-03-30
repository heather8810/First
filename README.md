# First
Calculating Black-scholes formula


public class BlackScholes
    	{

        	public BlackScholes()
        	{
           	 
        	}
        	public bool errorCheck(double s, double x, double t, double r, double v)
        	{

            	if (s <= 0)
            	{
                	MessageBox.Show("Error : Stock price has to be greater than 0. ");
                	return false;
            	}
            	else if (x <= 0)
            	{
                	MessageBox.Show("Error: Strike Price has to be greater than 0.");
                	return false;
            	}
            	else if (t <= 0)
            	{
                	MessageBox.Show("Error : Select correct date." + Environment.NewLine + "Expiration Date should be later than Current Date.");
                	return false;
            	}
            	else if (r >= 1)
            	{
                	MessageBox.Show("Error: Rate has to be a whole number." + Environment.NewLine + "e.g., 0.08 instead of 8%.");
                	return false;
            	}
            	else if (v >= 1)
            	{
                	MessageBox.Show("Error : Volatility has to be a whole number." + Environment.NewLine + "e.g., 0.08 instead of 8%.");
                	return false;
            	}
            	else
                	return true;
             	 
        	}
        	public double blackScholes(string CallPutFlag, double S, double X,
            	double T, double r, double v)
        	{
       	 
            	double d1 = 0.0;
            	double d2 = 0.0;
            	double dBlackScholes = 0.0;

            	d1 = (Math.Log(S / X) + (r + v * v / 2.0) * T) / (v * Math.Sqrt(T));
            	d2 = d1 - v * Math.Sqrt(T);
            	if (CallPutFlag == "c")
            	{
                	dBlackScholes = S * CND(d1) - X * Math.Exp(-r * T) * CND(d2);
            	}
            	else if (CallPutFlag == "p")
            	{
                	dBlackScholes = X * Math.Exp(-r * T) * CND(-d2) - S * CND(-d1);
            	}
            	return dBlackScholes;
        	}

        	public double CND(double X)
        	{
            	double L = 0.0;
            	double K = 0.0;
            	double dCND = 0.0;
            	const double a1 = 0.31938153;
            	const double a2 = -0.356563782;
            	const double a3 = 1.781477937;
            	const double a4 = -1.821255978;
            	const double a5 = 1.330274429;
            	L = Math.Abs(X);
            	K = 1.0 / (1.0 + 0.2316419 * L);
            	dCND = 1.0 - 1.0 / Math.Sqrt(2 * Convert.ToDouble(Math.PI.ToString())) *
                	Math.Exp(-L * L / 2.0) * (a1 * K + a2 * K * K + a3 * Math.Pow(K, 3.0) +
                	a4 * Math.Pow(K, 4.0) + a5 * Math.Pow(K, 5.0));

            	if (X < 0)
            	{
                	return 1.0 - dCND;
            	}
            	else
            	{
                	return dCND;
            	}
        	}
    	}


        	public Form1()
        	{
            	InitializeComponent();
        	}
     	 
    	private void btnCalculate_Click(object sender, EventArgs e)
    	{
        	bool gotStockPrice, gotStrikePrice,gotRate,gotVol;
        	double s,x,r,v;

        	System.TimeSpan daysdiff = dtpExprDate.Value - dtpCurrentDate.Value;
        	double yearsdiff = ((double)daysdiff.Days / 365.0); // year difference
        	double t = yearsdiff;
      	 
        	gotStockPrice = Double.TryParse(tbStockPrice.Text, out s);// Stock Price
        	gotStrikePrice = Double.TryParse(tbStrikePrice.Text, out x);// Strike Price
        	gotRate = Double.TryParse(tbRate.Text, out r); // Risk Free Rate
        	gotVol = Double.TryParse(tbVolatility.Text, out v); // Volatibility,  sigma

        	BlackScholes test = new BlackScholes();

        	if (!gotStockPrice || !gotStrikePrice || !gotRate || !gotVol)
        	{
            	MessageBox.Show("You have to enter all the value." + Environment.NewLine + "(Stock Price, Strike Price, Risk Free Rate, Volatility )");
        	}
        	else
        	{
            	bool proceed = test.errorCheck(s, x, t, r, v);
            	if (proceed)
            	{

                	double callPrice = test.blackScholes("c", s, x, t, r, v);
                	double putPrice = test.blackScholes("p", s, x, t, r, v);

                	tbCallPrice.Text = callPrice.ToString(".0000");
                	// tbCallPrice.Text = Convert.ToString(callPrice);
                	tbPutPrice.Text = putPrice.ToString(".0000");
                	// tbPutPrice.Text = Convert.ToString(putPrice);
            	}
        	}
    	}

