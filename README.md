# c-code-to-levy-distribute
/* Levy distribution 
 * http://markread.info/2016/08/code-to-generate-a-levy-distribution/  
 */
#define PI 3.14159265358979323846

/* Samples a levy distribution wherein the power law decay can be 
 * adjusted between 1/x and 1/x^3.
 * Note that this sampling method can return negative value. Values 
 * are symmetrical around zero.
 * param mu must lie between 1 and 3. Correspods to 1/x and 1/x^3*/
double
sample(double mu)
{
	double u,v,t,s;
	double alpha=mu-1;
		
	if ( (mu<=1)||(mu>3))
	return 0;

	do {
		u=(double)rand() / RAND_MAX;	
		v=(double)rand() / RAND_MAX;
	} while(!v);				// discard 0
	u=PI*(u-0.5);				// -PI/2 < u < PI/2	
	v=-log(v);				

	//general case
	t =  sin(alpha*u)/pow(cos(u),1/alpha);
	s =  pow(cos((1-alpha)*u)/v,(1-alpha)/alpha);
	return t*s;
}

/* Same as,but ensure all values are positive. Negative value are simple
 * negated, as the levy distribution represented is symmetrical 
 * around zero .*/
double 
randlevy(double mu,double scale)
{
	double l=scale*sample(mu);
	if (l<0.0) 
		return -1.0 * l;
	return l;
}

/* Default value case,scale=1*/
double 
randl(double mu)
{
	return randlevy(mu,1.0);
}
