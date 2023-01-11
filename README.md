public JsonResult Post(Patient p)
        {
            string query = @"
              select insertData(@FirstName,@LastName,@MiddleName,@Sex,@Dob::date);
            ";

            DataTable table = new DataTable();
            string sqlDataSource = _configuration.GetConnectionString("CrudApi");
            NpgsqlDataReader myReader;
            using (NpgsqlConnection myCon = new NpgsqlConnection(sqlDataSource))
            {
                myCon.Open();
                using (TransactionScope transactionScope = new TransactionScope())
                {
                    try
                    {
                        using (NpgsqlCommand myCommand = new NpgsqlCommand(query, myCon))
                        {
                            myCommand.Parameters.AddWithValue("@FirstName", p.FirstName);
                            myCommand.Parameters.AddWithValue("@LastName", p.LastName);
                            myCommand.Parameters.AddWithValue("@MiddleName", p.MiddleName);
                            myCommand.Parameters.AddWithValue("@Sex", p.Sex);
                            myCommand.Parameters.AddWithValue("@Dob", p.Dob);


                            myReader = myCommand.ExecuteReader();
                            table.Load(myReader);

                            myReader.Close();
                            myCon.Close();

                        }

                        transactionScope.Complete();
                        transactionScope.Dispose();
                    }
                    catch (TransactionException ex)
                    {
                        transactionScope.Dispose();

                    }
                }
            }
               
                return new JsonResult(("record inserted"));
        }
