####update

	def update
    	# 'delete and insert'
    	@@movie_db.delete_if do |m|
      		m["imdbID"] == params[:id]
    	end

    	#create new movie
    	movie = params.require(:movie).permit(:title, :year)
    	movie['imdbID'] = params[:id]

    	@@movie_db << movie
    	redirect_to action: :index
	end  	

####destroy

	def destroy
    	@@movie_db.delete_if do |m|
      		m["imdbID"] == params[:id]
    	end
    	redirect_to action: :index
  	end