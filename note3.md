


    rails generate controller Destinations

Capital letter, plural form

      get '/destinations/:id' => 'destinations#show', as: :destination

then in `controller`

      def show
        @destination = Destination.find(params[:id])
      end



      <%= image_tag @destination.image %>
      <h2><%= @destination.name %></h2>
      <p><%= @destination.description %></p>
      <p><%= link_to "See More", destination_path(@destination.id) %></p>



      get '/destinations/:id/edit' => 'destinations#edit', as: :edit_destination 
      patch '/destinations/:id' => 'destinations#update'


in `destinations_controller`



    def update 
      @destination = Destination.find(params[:id]) 
      if @destination.update_attributes(destination_params) 
        redirect_to(:action => 'show', :id => @destination.id) 
      else 
        render 'edit' 
      end 
    end
    private 
      def destination_params 
        params.require(:destination).permit(:name, :description) 
      end