# DiscountCalculator
import React, { useState } from 'react';
import { StyleSheet,Text, View, TouchableOpacity, TextInput, Button} from "react-native";
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';   
import { DataTable} from 'react-native-paper';
import DrawerLayout from 'react-native-gesture-handler/DrawerLayout';


var a = [];
var b = [];
const Stack = createStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator 
      initialRouteName="HOMESCREN"
      screenOptions ={{
        headerTitleAlign:"center",
            headerTintColor:'green',
            headerStyle: {
              backgroundColor:'blue',

            }
      }

      }
      >
        <Stack.Screen 
        name="HOMESCREN" 
        component={HS}
        options={
          {
            title:"WELCOME",
            headerTintColor:'yellow',
             headerStyle: {
               backgroundColor:'#374a46',

             },
        

          }
          
        }
         />
        <Stack.Screen 
        name="START" 
        component={START}
        options={({ navigation }) => ({
          title: "Discount App",
          headerTitleAlign: 'center',
          headerTintColor: 'green',
          headerStyle: {
            backgroundColor:'#30403c',
          },
          headerRight: () => (
            <Button
              title="HISTORY"
              onPress={() => navigation.navigate('COMP', {history:h})}
              color='red'
            ><Text></Text></Button>
            
            )
        })}
      
          
        />
        <Stack.Screen 
        name="COMP" 
        component={COMP}
        options={
          {
            title:"HISTORY",
            headerTintColor:'red',
             headerStyle: {
               backgroundColor:'#435450',

             },
        

          }
          
        }
         />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

const HS = ({navigation}) => {
  return (
    <View style={styles.container}>
      <TouchableOpacity 
      style={styles.b1}
      activeOpacity={0.1} 
      onPress={() => navigation.navigate('START')}>
      <Text>LET'S START GUYS!</Text>
      </TouchableOpacity>
    </View>
  );
}

const START = ({navigation}) => {
  const [price,setNewPrice]=useState('');
  const [t,setT]=useState(/^[0-9\b]+$/);
  const [discountPercent,setDiscountPercent]=useState('');
  const [discountedPrice,setDiscountedPrice]=useState('');
  const [discountResult, setDiscountResult] = useState('');
  const [saveButton,setSaveButton]=useState(false);
  const [clearButton,setClearButton]=useState(true);
  const [HISTORY,setHISTORY] = useState([]);
  var array = [price,discountPercent,discountedPrice];

  
  const handlePriceInput=(text)=>{
    setNewPrice(text)
    if(!t.test(text)){
      alert('ENTER POSTIVE NUMBER ONLY')
      setNewPrice('')
    }
  }

  const handleDiscountInput=(text)=>{
    setDiscountPercent(text)
    if(!t.test(text)){
      alert('ENTER POSTIVE NUMBER ONLYy')
      setDiscountPercent('')
    }
    else if(text >=100){
      alert('Discount Greater than 100% is not possible')
      setDiscountPercent('')

    }
  }
    
    

  

  const clearText=()=>{
    setNewPrice('')
    setDiscountPercent('')
    setDiscountedPrice('')
    setDiscountResult('')
    setSaveButton(false)
    setClearButton(true)
    
  }

  const discountFunc =()=>{
    setClearButton(false)
    if(price==''){
      alert('ENTER ITEM PRICE')
    }
    else if(discountPercent==''){
      alert('FIRST ENTER DISCOUNT PERCENTAGE')

    }
    else if(price!='' && discountPercent!=''){
      var dis_count = (price -(price * discountPercent/100))
      var totalSave = (price - dis_count)
      var discount = dis_count.toFixed(2);
      var totalDiscount =totalSave.toFixed(2);
      setDiscountedPrice(discountedPrice+discount)
      setDiscountResult(totalDiscount);
    }
  }


  const addToHISTORY=()=>{

    
    if(price!=="" && discountPercent !=="" && discountedPrice !==""){
      var newDatawArray = history.slice()
      newDatawArray.push(array)
      setHISTORY(newDatawArray)
      h = newDatawArray
      alert('New Data Added to HISTORY')
      setSaveButton(true)

  }
    else{
      alert('FIRST CALCULATE DISCOUNT'),
      console.log("PLEASE ENTER THE DATA FIRST")
    }

    }

  return (
    <View style={styles.container}>
      
      <View>
        <View>
        <TextInput 
        style={styles.TextComponentStyle} 
        onChangeText={(text)=> handlePriceInput(text)}
        placeholder="PLEASE ENTER THE PRICE"
        value={price}
        >
          
        </TextInput>
          
        </View>

        <View >
        <TextInput
        style={styles.TextComponentStyle}
        onChangeText={text=>handleDiscountInput(text)}
        placeholder="PLEASE ENTER PERCENTAGE OF DISCOUNT"
        value={discountPercent}
        ></TextInput>
        </View>
      </View>

      <View style={styles.saveAndDiscountBox}>
          <Text style={styles.textStyle} >  DISCOUNTED PRICE </Text>
          <TextInput style={styles.TextComponentStyle}><Text></Text>{discountedPrice}</TextInput>
          <Text style={styles.textStyle}>  You Saved</Text>
          <TextInput style={styles.TextComponentStyle}>{discountResult}</TextInput>  
          </View>
      <View style={styles.buttonstyle}>

      </View>

      <View style={styles.buttonstyle}>
        <View>
        <TouchableOpacity 
        style={styles.calculateStyle}
        activeOpacity={0.1}
        onPress={discountFunc}>
        <Text 
        style={styles.buttonText}>CALCULATE</Text>  
        </TouchableOpacity>
        </View>
        <View >
        <TouchableOpacity   
        style={styles.saveColor}
        activeOpacity={0.1}
        disabled={saveButton} 
        onPress={addToHISTORY}
        >
        <Text style={styles.buttonText}>SAVE</Text></TouchableOpacity>
        </View>
        <View>
          
        <TouchableOpacity   
        style={styles.saveColor}
        activeOpacity={0.1} 
        onPress={()=>{clearText()}}
        disabled={clearButton}
        >
        <Text style={styles.buttonText}>CLEAR</Text></TouchableOpacity>
        </View>
      </View>
      
    </View>

  );
}
const COMP = ({route}) => {
  var ar=route.params.history;
  const [list,setList]=useState(ar);

  console.log(list);

  const removeItem =(index)=>{
    let NewArray = [...list];
    NewArray.splice(index, 1);
    setList(NewArray);

  }

  const clearItems =()=>{
    setList(c);

  }

  return (
    <View>
      <View>
      <TouchableOpacity   
        style={styles.clear}
        activeOpacity={0.1} 
        onPress={()=>{clearItems()}}
        //disabled={clearButton}
        >
        <Text style={styles.buttonText}>CLEAR</Text></TouchableOpacity>
      </View>
  
    <DataTable>
        <DataTable.Header>
            <DataTable.Title >Serial.No</DataTable.Title>
            <DataTable.Title >Rate</DataTable.Title>
            <DataTable.Title >Discount</DataTable.Title>
            <DataTable.Title >Final Price</DataTable.Title>
            
        </DataTable.Header>
        
        {list.map(((values, index) =>
            <DataTable.Row>
                <DataTable.Cell >{index + 1}</DataTable.Cell>
                <DataTable.Cell >    {values[0]}</DataTable.Cell>
                <DataTable.Cell>       {values[1]}% </DataTable.Cell>
                <DataTable.Cell numeric>{values[2]}</DataTable.Cell>
                <DataTable.Cell ><Button
                    title="X"          
                    onPress={() => removeItem(list.index)}></Button>
                </DataTable.Cell>
            </DataTable.Row>
        ))}
    </DataTable>
</View>
  );
}

  const styles = StyleSheet.create({
  container: {
    flex: 1,
    width:"100%",
    backgroundColor:"black",
    alignItems: "center",
    justifyContent: "center",
    backgroundColor:'#008B8B',

  },
  textStyle: {
    fontWeight:'bold',
    color:'red',
  },

  buttonstyle: {
    flexDirection: "row",
    alignItems:"center",
    alignContent:'center',
    flexDirection: "row",
    justifyContent:'space-between',
    margin:2
  },

buttonText:{
    color:'#fff',
    textAlign:"center",
    fontWeight:'bold',
    //width:100,
},
clear: {
  margin:15,
  backgroundColor:'yellow',
  padding:8,
  width:85,
  borderRadius:10,
  marginLeft:290,
},
saveColor:{
  paddingTop:10,
  paddingBottom:10,
  backgroundColor:'#800000',
  borderRadius:10,
  borderWidth: 1,
  borderColor: 'red',
  marginLeft:2,
  width:70
},
calculateStyle:{
  padding:10,
  backgroundColor:'#483D8B',
  borderRadius:10,
  borderWidth: 1,
  borderColor: 'red',
  marginRight:2,
  //width:70

},
b1: {
  backgroundColor:'lightblue',
  borderRadius:10,
  padding:10,
},

numberStyle2:{
  marginRight:10,
  alignItems:'center',
  padding:8,
  width:80,
  backgroundColor:'orange',
  borderRadius:10,
  borderWidth:1,
  borderColor: 'red'
  
  },
TextComponentStyle: {
  padding:10,
  width:250,
  height:50,
  borderWidth: 2,
  borderRadius:10,
  borderColor: '#0808c7',
  color: 'red',
  fontWeight:'italic',
  backgroundColor : 'red',
  fontSize: 10,
  textAlign: 'center',
  margin: 10
},

});


export default App;
