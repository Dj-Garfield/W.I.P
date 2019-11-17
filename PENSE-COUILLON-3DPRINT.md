### Calculate how many steps for a given diameter of gear for an extrudeur direct-drive ( not Geared )

 steps_per_mm = (pas_moteur_par_tour * nb_de_micropas_driver) / (diamètre_effectif_galet * pi)

exemple : (400 pas moteur * 16 micropas) / (diamètre galet 12mm * 3.14159) = 85 steps_per_mm 

### Calculate how many steps for a given M8 lead screw for Z axis

 steps_per_mm = (pas_moteur_par_tour * nb_de_micropas_driver) / pas_du_filetage

exemple : (400 pas moteur * 16 micropas) / (pas de filetage 8mm) = 800 steps_per_mm 

### Calculate how many steps for a given gear for a belt drive carrier

 steps_per_mm = (pas_moteur_par_tour * nb_de_micropas_driver) / (pas_courroie * nombre_de_dents_de_la_poulie)

exemple : (200 pas moteur * 16 micropas) / (courroie 2mm * poulie 16 dents) = 100 steps_per_mm 
