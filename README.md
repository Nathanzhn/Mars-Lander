# Mars-Lander-c++

	double LANDER_MASS;
	vector3d a, F, W, T, D_Lander, D_Parachute;

	LANDER_MASS = UNLOADED_LANDER_MASS + FUEL_CAPACITY*FUEL_DENSITY*fuel;
	W = -GRAVITY*MARS_MASS*LANDER_MASS*position.norm() / (position.abs2());
	
	D_Lander = -0.5*DRAG_COEF_LANDER*atmospheric_density(position)*M_PI*LANDER_SIZE*LANDER_SIZE*velocity.abs2()*velocity.norm();
	D_Parachute = -0.5*DRAG_COEF_CHUTE*atmospheric_density(position)*5.0*2.0*LANDER_SIZE*2.0*LANDER_SIZE*velocity.abs2()*velocity.norm();
	T = thrust_wrt_world();
	//cout << "" << "  " << position << "  " << position.abs() << "  " << velocity.abs() << " " << velocity << endl;
	if (parachute_status == DEPLOYED) {
		F = W + T + D_Lander + D_Parachute;
	}
	else {
		F = W + T + D_Lander;
	}
	
	a = F / LANDER_MASS;

	/*
	//euler integrator
	position = position + velocity*delta_t;
	velocity = velocity + a*delta_t;
	*/
	
	//verlet integrator
	static vector3d previous_position;
	vector3d new_position;

	if (simulation_time == 0.0) {
		new_position = position + velocity*delta_t;
		velocity += a*delta_t;
	}
	else {
		new_position = 2 * position - previous_position + delta_t*delta_t*a;
		velocity = (new_position - position) / (delta_t);
	}

	previous_position = position;
	position = new_position;
	
