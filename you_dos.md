
##YOU DOs:

###End of: Password and Password confirmation

> rails c
> 
> User.create(name: "Marky Mark", email: "marky@example.com",password: "foobar", password_confirmation: "foobar")


###Views

**create views/users/new.html.erb**


	<h1>Sign up</h1>

	<div class="row">
  		<div class="span6 offset3">
    		<%= form_for(@user) do |f| %>
      			<% unless @user.errors.count == 0   %>
          		<div class="alert alert-error">
            		The form contains <%= pluralize(@user.errors.count, "error") %>.
          		</div>
        		<ul>
        		<% @user.errors.full_messages.each do |msg| %>
            		<li>* <%= msg %></li>
        		<% end %>
       			</ul>
        		<% end %>
        		
      			<%= f.label :name %>
      			<%= f.text_field :name %>

      			<%= f.label :email %>
      			<%= f.text_field :email %>

      			<%= f.label :password %>
      			<%= f.password_field :password %>

      			<%= f.label :password_confirmation, "Confirmation" %>
      			<%= f.password_field :password_confirmation %>

      			<%= f.submit "Create my account", class: "btn btn-large btn-primary" %>
    		<% end %>
  		</div>
	</div>


**create views/users/show.html.erb** (profile page)

	<div class="row">
  		<aside class="span4">
    		<section>
      			<h3><%= @user.name %></h3>
      			<h3><%= @user.email %></h3>
    		</section>
  		</aside>
	</div>
	
	
**create views/sessions/new.html.erb** (sign in page)


	<h1>Sign in</h1>

	<div class="row">
  		<div class="span6 offset3">
    		<%= form_for(:session, url: sessions_path) do |f| %>

      		<%= f.label :email %>
      		<%= f.text_field :email %>

      		<%= f.label :password %>
      		<%= f.password_field :password %>

      		<%= f.submit "Sign in", class: "btn btn-large btn-primary" %>
    	<% end %>

    	<p>New user? <%= link_to "Sign up now!", signup_path %></p>
  		</div>
	</div>


###Controllers

	

