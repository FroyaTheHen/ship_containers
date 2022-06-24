-- public.ship_containers_country definition

-- Drop table

-- DROP TABLE public.ship_containers_country;

CREATE TABLE public.ship_containers_country (
	id uuid NOT NULL,
	country varchar(32) NOT NULL,
	CONSTRAINT ship_containers_country_pkey PRIMARY KEY (id)
);


-- public.ship_containers_personbasemodel definition

-- Drop table

-- DROP TABLE public.ship_containers_personbasemodel;

CREATE TABLE public.ship_containers_personbasemodel (
	id uuid NOT NULL,
	first_name varchar(32) NOT NULL,
	last_name varchar(32) NOT NULL,
	birth_date date NOT NULL,
	CONSTRAINT ship_containers_personbasemodel_pkey PRIMARY KEY (id)
);


-- public.ship_containers_shipment definition

-- Drop table

-- DROP TABLE public.ship_containers_shipment;

CREATE TABLE public.ship_containers_shipment (
	id uuid NOT NULL,
	estimated_arrival timestamptz NOT NULL,
	arrival timestamptz NULL,
	"USD_value" numeric(24, 8) NOT NULL,
	created_at timestamptz NOT NULL,
	CONSTRAINT ship_containers_shipment_pkey PRIMARY KEY (id)
);


-- public.ship_containers_company definition

-- Drop table

-- DROP TABLE public.ship_containers_company;

CREATE TABLE public.ship_containers_company (
	id uuid NOT NULL,
	"name" varchar(32) NOT NULL,
	establishment_date date NOT NULL,
	created_at timestamptz NOT NULL,
	updated_at timestamptz NOT NULL,
	country_id uuid NOT NULL,
	CONSTRAINT ship_containers_company_pkey PRIMARY KEY (id),
	CONSTRAINT ship_containers_comp_country_id_d210dd6f_fk_ship_cont FOREIGN KEY (country_id) REFERENCES public.ship_containers_country(id) DEFERRABLE INITIALLY DEFERRED
);
CREATE INDEX ship_containers_company_ensign_id_c9ef83a7 ON public.ship_containers_company USING btree (country_id);


-- public.ship_containers_employee definition

-- Drop table

-- DROP TABLE public.ship_containers_employee;

CREATE TABLE public.ship_containers_employee (
	personbasemodel_ptr_id uuid NOT NULL,
	company_id_id uuid NOT NULL,
	CONSTRAINT ship_containers_employee_pkey PRIMARY KEY (personbasemodel_ptr_id),
	CONSTRAINT ship_containers_empl_company_id_id_714b78f2_fk_ship_cont FOREIGN KEY (company_id_id) REFERENCES public.ship_containers_company(id) DEFERRABLE INITIALLY DEFERRED,
	CONSTRAINT ship_containers_empl_personbasemodel_ptr__443cdd0c_fk_ship_cont FOREIGN KEY (personbasemodel_ptr_id) REFERENCES public.ship_containers_personbasemodel(id) DEFERRABLE INITIALLY DEFERRED
);
CREATE INDEX ship_containers_employee_company_id_id_714b78f2 ON public.ship_containers_employee USING btree (company_id_id);


-- public.ship_containers_harbor definition

-- Drop table

-- DROP TABLE public.ship_containers_harbor;

CREATE TABLE public.ship_containers_harbor (
	id uuid NOT NULL,
	latitude numeric(9, 6) NOT NULL,
	longitude numeric(9, 6) NOT NULL,
	city varchar(128) NOT NULL,
	country_id uuid NOT NULL,
	CONSTRAINT ship_containers_harbor_pkey PRIMARY KEY (id),
	CONSTRAINT ship_containers_harb_country_id_0b3983d3_fk_ship_cont FOREIGN KEY (country_id) REFERENCES public.ship_containers_country(id) DEFERRABLE INITIALLY DEFERRED
);
CREATE INDEX ship_containers_harbor_country_id_0b3983d3 ON public.ship_containers_harbor USING btree (country_id);


-- public.ship_containers_loadingbasemodel definition

-- Drop table

-- DROP TABLE public.ship_containers_loadingbasemodel;

CREATE TABLE public.ship_containers_loadingbasemodel (
	id uuid NOT NULL,
	created_at timestamptz NOT NULL,
	updated_at timestamptz NOT NULL,
	shipment_id uuid NOT NULL,
	responsible_employee_id uuid NOT NULL,
	CONSTRAINT ship_containers_loadingbasemodel_pkey PRIMARY KEY (id),
	CONSTRAINT ship_containers_load_responsible_employee_df5d4a0a_fk_ship_cont FOREIGN KEY (responsible_employee_id) REFERENCES public.ship_containers_employee(personbasemodel_ptr_id) DEFERRABLE INITIALLY DEFERRED,
	CONSTRAINT ship_containers_load_shipment_id_d2389547_fk_ship_cont FOREIGN KEY (shipment_id) REFERENCES public.ship_containers_shipment(id) DEFERRABLE INITIALLY DEFERRED
);
CREATE INDEX ship_containers_loadingbas_responsible_employee_id_df5d4a0a ON public.ship_containers_loadingbasemodel USING btree (responsible_employee_id);
CREATE INDEX ship_containers_loadingbasemodel_shipment_id_d2389547 ON public.ship_containers_loadingbasemodel USING btree (shipment_id);


-- public.ship_containers_privatecontainerowner definition

-- Drop table

-- DROP TABLE public.ship_containers_privatecontainerowner;

CREATE TABLE public.ship_containers_privatecontainerowner (
	personbasemodel_ptr_id uuid NOT NULL,
	citizenship_id uuid NOT NULL,
	CONSTRAINT ship_containers_privatecontainerowner_pkey PRIMARY KEY (personbasemodel_ptr_id),
	CONSTRAINT ship_containers_priv_citizenship_id_ed21992a_fk_ship_cont FOREIGN KEY (citizenship_id) REFERENCES public.ship_containers_country(id) DEFERRABLE INITIALLY DEFERRED,
	CONSTRAINT ship_containers_priv_personbasemodel_ptr__1e62f3b7_fk_ship_cont FOREIGN KEY (personbasemodel_ptr_id) REFERENCES public.ship_containers_personbasemodel(id) DEFERRABLE INITIALLY DEFERRED
);
CREATE INDEX ship_containers_privatecontainerowner_citizenship_id_ed21992a ON public.ship_containers_privatecontainerowner USING btree (citizenship_id);


-- public.ship_containers_ship definition

-- Drop table

-- DROP TABLE public.ship_containers_ship;

CREATE TABLE public.ship_containers_ship (
	id uuid NOT NULL,
	container_capacity int4 NOT NULL,
	"name" varchar(32) NOT NULL,
	ensign_id uuid NOT NULL,
	CONSTRAINT ship_containers_ship_pkey PRIMARY KEY (id),
	CONSTRAINT ship_containers_ship_ensign_id_f6a18f75_fk_ship_cont FOREIGN KEY (ensign_id) REFERENCES public.ship_containers_country(id) DEFERRABLE INITIALLY DEFERRED
);
CREATE INDEX ship_containers_ship_ensign_id_f6a18f75 ON public.ship_containers_ship USING btree (ensign_id);


-- public.ship_containers_shipcheckin definition

-- Drop table

-- DROP TABLE public.ship_containers_shipcheckin;

CREATE TABLE public.ship_containers_shipcheckin (
	id uuid NOT NULL,
	arrival timestamptz NOT NULL,
	departure date NOT NULL,
	harbour_id uuid NOT NULL,
	ship_id uuid NOT NULL,
	CONSTRAINT ship_containers_shipcheckin_pkey PRIMARY KEY (id),
	CONSTRAINT ship_containers_ship_harbour_id_29c08fdc_fk_ship_cont FOREIGN KEY (harbour_id) REFERENCES public.ship_containers_harbor(id) DEFERRABLE INITIALLY DEFERRED,
	CONSTRAINT ship_containers_ship_ship_id_be6bacfa_fk_ship_cont FOREIGN KEY (ship_id) REFERENCES public.ship_containers_ship(id) DEFERRABLE INITIALLY DEFERRED
);
CREATE INDEX ship_containers_shipcheckin_harbour_id_29c08fdc ON public.ship_containers_shipcheckin USING btree (harbour_id);
CREATE INDEX ship_containers_shipcheckin_ship_id_be6bacfa ON public.ship_containers_shipcheckin USING btree (ship_id);


-- public.ship_containers_container definition

-- Drop table

-- DROP TABLE public.ship_containers_container;

CREATE TABLE public.ship_containers_container (
	id uuid NOT NULL,
	cubic_capacity int4 NOT NULL,
	carrying_capacity int4 NOT NULL,
	company_owner_id uuid NULL,
	private_owner_id uuid NULL,
	CONSTRAINT ship_containers_container_pkey PRIMARY KEY (id),
	CONSTRAINT ship_containers_cont_company_owner_id_51b2c19a_fk_ship_cont FOREIGN KEY (company_owner_id) REFERENCES public.ship_containers_company(id) DEFERRABLE INITIALLY DEFERRED,
	CONSTRAINT ship_containers_cont_private_owner_id_be2df2ea_fk_ship_cont FOREIGN KEY (private_owner_id) REFERENCES public.ship_containers_privatecontainerowner(personbasemodel_ptr_id) DEFERRABLE INITIALLY DEFERRED
);
CREATE INDEX ship_containers_container_company_owner_id_51b2c19a ON public.ship_containers_container USING btree (company_owner_id);
CREATE INDEX ship_containers_container_private_owner_id_be2df2ea ON public.ship_containers_container USING btree (private_owner_id);


-- public.ship_containers_container_authorized_company definition

-- Drop table

-- DROP TABLE public.ship_containers_container_authorized_company;

CREATE TABLE public.ship_containers_container_authorized_company (
	id bigserial NOT NULL,
	container_id uuid NOT NULL,
	company_id uuid NOT NULL,
	CONSTRAINT ship_containers_containe_container_id_company_id_b8a3f8c8_uniq UNIQUE (container_id, company_id),
	CONSTRAINT ship_containers_container_authorized_company_pkey PRIMARY KEY (id),
	CONSTRAINT ship_containers_cont_company_id_85985946_fk_ship_cont FOREIGN KEY (company_id) REFERENCES public.ship_containers_company(id) DEFERRABLE INITIALLY DEFERRED,
	CONSTRAINT ship_containers_cont_container_id_1e93a88d_fk_ship_cont FOREIGN KEY (container_id) REFERENCES public.ship_containers_container(id) DEFERRABLE INITIALLY DEFERRED
);
CREATE INDEX ship_containers_container__company_id_85985946 ON public.ship_containers_container_authorized_company USING btree (company_id);
CREATE INDEX ship_containers_container__container_id_1e93a88d ON public.ship_containers_container_authorized_company USING btree (container_id);


-- public.ship_containers_container_authorized_private_person definition

-- Drop table

-- DROP TABLE public.ship_containers_container_authorized_private_person;

CREATE TABLE public.ship_containers_container_authorized_private_person (
	id bigserial NOT NULL,
	container_id uuid NOT NULL,
	privatecontainerowner_id uuid NOT NULL,
	CONSTRAINT ship_containers_containe_container_id_privatecont_b89d1963_uniq UNIQUE (container_id, privatecontainerowner_id),
	CONSTRAINT ship_containers_container_authorized_private_person_pkey PRIMARY KEY (id),
	CONSTRAINT ship_containers_cont_container_id_40f4dd0b_fk_ship_cont FOREIGN KEY (container_id) REFERENCES public.ship_containers_container(id) DEFERRABLE INITIALLY DEFERRED,
	CONSTRAINT ship_containers_cont_privatecontainerowne_620b9e75_fk_ship_cont FOREIGN KEY (privatecontainerowner_id) REFERENCES public.ship_containers_privatecontainerowner(personbasemodel_ptr_id) DEFERRABLE INITIALLY DEFERRED
);
CREATE INDEX ship_containers_container__container_id_40f4dd0b ON public.ship_containers_container_authorized_private_person USING btree (container_id);
CREATE INDEX ship_containers_container__privatecontainerowner_id_620b9e75 ON public.ship_containers_container_authorized_private_person USING btree (privatecontainerowner_id);


-- public.ship_containers_loadingauthorization definition

-- Drop table

-- DROP TABLE public.ship_containers_loadingauthorization;

CREATE TABLE public.ship_containers_loadingauthorization (
	loadingbasemodel_ptr_id uuid NOT NULL,
	sender_company_id uuid NULL,
	sender_private_container_owner_id uuid NULL,
	CONSTRAINT ship_containers_loadingauthorization_pkey PRIMARY KEY (loadingbasemodel_ptr_id),
	CONSTRAINT ship_containers_load_loadingbasemodel_ptr_a38a5b81_fk_ship_cont FOREIGN KEY (loadingbasemodel_ptr_id) REFERENCES public.ship_containers_loadingbasemodel(id) DEFERRABLE INITIALLY DEFERRED,
	CONSTRAINT ship_containers_load_sender_company_id_a98a21fa_fk_ship_cont FOREIGN KEY (sender_company_id) REFERENCES public.ship_containers_company(id) DEFERRABLE INITIALLY DEFERRED,
	CONSTRAINT ship_containers_load_sender_private_conta_b903e300_fk_ship_cont FOREIGN KEY (sender_private_container_owner_id) REFERENCES public.ship_containers_privatecontainerowner(personbasemodel_ptr_id) DEFERRABLE INITIALLY DEFERRED
);
CREATE INDEX ship_containers_loadingaut_sender_private_container_o_b903e300 ON public.ship_containers_loadingauthorization USING btree (sender_private_container_owner_id);
CREATE INDEX ship_containers_loadingauthorization_sender_company_id_a98a21fa ON public.ship_containers_loadingauthorization USING btree (sender_company_id);


-- public.ship_containers_receivementauthorization definition

-- Drop table

-- DROP TABLE public.ship_containers_receivementauthorization;

CREATE TABLE public.ship_containers_receivementauthorization (
	loadingbasemodel_ptr_id uuid NOT NULL,
	loading_id uuid NOT NULL,
	receiving_company_id uuid NULL,
	receiving_private_container_owner_id uuid NULL,
	CONSTRAINT ship_containers_receivementauthorization_pkey PRIMARY KEY (loadingbasemodel_ptr_id),
	CONSTRAINT ship_containers_rece_loading_id_17a3620a_fk_ship_cont FOREIGN KEY (loading_id) REFERENCES public.ship_containers_loadingauthorization(loadingbasemodel_ptr_id) DEFERRABLE INITIALLY DEFERRED,
	CONSTRAINT ship_containers_rece_loadingbasemodel_ptr_2062ac55_fk_ship_cont FOREIGN KEY (loadingbasemodel_ptr_id) REFERENCES public.ship_containers_loadingbasemodel(id) DEFERRABLE INITIALLY DEFERRED,
	CONSTRAINT ship_containers_rece_receiving_company_id_58df6ef6_fk_ship_cont FOREIGN KEY (receiving_company_id) REFERENCES public.ship_containers_company(id) DEFERRABLE INITIALLY DEFERRED,
	CONSTRAINT ship_containers_rece_receiving_private_co_74fa107c_fk_ship_cont FOREIGN KEY (receiving_private_container_owner_id) REFERENCES public.ship_containers_privatecontainerowner(personbasemodel_ptr_id) DEFERRABLE INITIALLY DEFERRED
);
CREATE INDEX ship_containers_receivemen_receiving_company_id_58df6ef6 ON public.ship_containers_receivementauthorization USING btree (receiving_company_id);
CREATE INDEX ship_containers_receivemen_receiving_private_containe_74fa107c ON public.ship_containers_receivementauthorization USING btree (receiving_private_container_owner_id);
CREATE INDEX ship_containers_receivementauthorization_loading_id_17a3620a ON public.ship_containers_receivementauthorization USING btree (loading_id);